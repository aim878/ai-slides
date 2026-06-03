# Fine-Tuning Frontier LLMs

---

## Slide 1: Welcome to Fine-Tuning
- **What is Fine-Tuning?**
  - Customizing a pre-trained model for specific tasks
  - Creating your own private version of a model
  - Teaching the model your specific style, format, or domain

**Key Point**: Fine-tuning adapts existing knowledge, not teaching from scratch

---

## Slide 2: LoRA - Low Rank Adapters
- **What is LoRA?**
  - **Lo**w **R**ank **A**dapters
  - Trains a small set of parameters, not the entire model
  - These parameters "nudge" the big model's behavior

**Analogy**: Like adding a specialized lens to a camera - modifies output without rebuilding the camera

**Note**: OpenAI likely uses LoRA internally (not disclosed)

---

## Slide 3: Three Stages of Fine-Tuning
**Stage 1: Create Training Data**
- Format data as JSONL (JSON Lines)
- Each line = one training example
- Upload to OpenAI

**Stage 2: Run Training**
- Monitor loss metrics
- Watch accuracy improve
- Training happens in OpenAI's cloud

**Stage 3: Evaluate & Iterate**
- Test the fine-tuned model
- Tweak hyperparameters
- Repeat if needed

---

## Slide 4: OpenAI Fine-Tuning Options
**Three Types Available**:

1. **SFT** - Supervised Fine-Tuning (most common)
2. **DPO** - Direct Preference Optimization
3. **RFT** - Reinforcement Fine-Tuning

**Our Focus**: SFT (Supervised Fine-Tuning)

---

## Slide 5: Supervised Fine-Tuning (SFT)
**What It Does**:
- Provide input → correct output pairs
- Model learns from labeled examples
- Customizes style, tone, and format

**Best For**:
- Classification tasks
- Translation with nuance
- Generating content in a specific style
- Correcting consistent failures

**Models**: GPT-4.1, GPT-4.1-mini, GPT-4.1-nano

---

## Slide 6: Direct Preference Optimization (DPO)
**What It Does**:
- Learn from good vs. bad examples
- Users give thumbs up/down feedback
- Refines based on preferences

**Best For**:
- Adjusting style and tone
- Subjective improvements
- Nuanced behavior changes

**Successor to**: RLHF (Reinforcement Learning from Human Feedback)

---

## Slide 7: Reinforcement Fine-Tuning (RFT)
**What It Does**:
- Uses a "grader" to evaluate responses
- Grader can be another LLM ("LLM as judge")
- Shifts behavior based on grades

**Best For**:
- Expert-level performance
- Nuanced objectives
- When you don't have labeled data

**Requirement**: Only works with reasoning models (e.g., o1-mini)

---

## Slide 8: When to Use Each Type
| Type | Use When | Example |
|------|----------|---------|
| **SFT** | You have labeled data | Product classification |
| **DPO** | You have preference data | Style refinement |
| **RFT** | You can grade outputs | Medical diagnosis |

**Most Common**: SFT (Supervised Fine-Tuning)

---

## Slide 9: What You Get
**Your Private Model**:
- Custom version of base model
- Only accessible to you
- Small set of parameters stored
- Applied to base model at inference

**Not**: A complete copy of GPT
**Actually**: Small adapter parameters (LoRA)

---

## Slide 10: Data Format - JSONL
**JSON Lines Format**:
```
{"messages": [{"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
{"messages": [{"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
```

**Each Line**:
- One training example
- User message (input)
- Assistant message (expected output)

**Similar to**: Batch API format

---

## Slide 11: How Much Data?
**OpenAI Recommendation**:
- Start with **50-100 examples**
- Only increase if needed
- More ≠ always better

**Why So Few?**:
- Frontier models already have massive knowledge
- Fine-tuning adds "hints" not core knowledge
- Teaching format/style needs few examples

**Cost**: Very affordable with nano models

---

## Slide 12: Training Process
**Step 1**: Upload training file
```python
client.files.create(
    file=open("training.jsonl", "rb"),
    purpose="fine-tune"
)
```

**Step 2**: Create fine-tuning job
```python
client.fine_tuning.jobs.create(
    training_file=file_id,
    model="gpt-4.1-nano"
)
```

**Step 3**: Monitor and retrieve

---

## Slide 13: Key Hyperparameters
**Epochs**:
- Number of passes through data
- Usually 1 epoch is enough (with lots of data)

**Batch Size**:
- Number of examples per training step
- Smaller = more precise, slower
- Larger = faster, less precise

**Learning Rate**:
- OpenAI auto-selects (usually)
- Controls how much to adjust

**Seed**: For reproducibility

---

## Slide 14: Monitoring Training
**Loss Metrics**:
- **Training Loss**: How well model fits training data
- **Validation Loss**: How well model generalizes

**What to Look For**:
- Loss decreasing over time
- Validation loss not increasing (overfitting)

**OpenAI Dashboard**: Visual charts of progress

**Platform**: https://platform.openai.com/finetune

---

## Slide 15: Validation Data
**Purpose**:
- Held-out data not used for training
- Unbiased measure of performance
- Detects overfitting

**Recommendation**:
- 50-100 validation examples
- Separate from training data
- Same format as training data

---

## Slide 16: When Fine-Tuning Works Well
**Good Use Cases**:
✅ Setting style or tone
✅ Improving output reliability
✅ Formatting (e.g., always return JSON)
✅ Correcting specific failures
✅ Handling edge cases
✅ Tasks hard to explain in prompts

**Example**: "Always respond with sarcasm" or "Refuse personal questions"

---

## Slide 17: When Fine-Tuning Doesn't Work
**Poor Use Cases**:
❌ Teaching completely new knowledge
❌ When prompt engineering works well
❌ Tasks requiring massive domain expertise
❌ When you have limited examples

**Better Alternative**: Use RAG or larger context

---

## Slide 18: Cost Considerations
**Pricing** (approximate):
- **Training**: ~$0.008 per 1K tokens (nano)
- **Inference**: Slightly more than base model
- **Storage**: Free for limited time

**Example**:
- 100 examples ≈ $0.10
- 20,000 examples ≈ $3-4

**Tip**: Start small, scale if needed

---

## Slide 19: The Fine-Tuning API Flow
1. **Prepare Data** → JSONL format
2. **Upload Files** → Training & validation
3. **Create Job** → Specify model & hyperparameters
4. **Monitor** → Watch loss metrics
5. **Retrieve Model** → Get fine-tuned model ID
6. **Test** → Run inference
7. **Iterate** → Adjust and repeat

---

## Slide 20: Testing Your Model
**Use Same Format**:
```python
response = client.chat.completions.create(
    model="ft:gpt-4.1-nano:personal:suffix:id",
    messages=[{"role": "user", "content": "..."}]
)
```

**Important**: Don't include assistant response in test!

**Common Mistake**: Including answer in test → perfect scores (false positive)

---

## Slide 21: Evaluation Metrics
**For Regression** (predicting numbers):
- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)

**For Classification**:
- Accuracy
- Precision/Recall
- F1 Score

**For Generation**:
- Human evaluation
- LLM-as-judge
- Task-specific metrics

---

## Slide 22: Interpreting Loss Charts
**Initial Drop**: Model learns format quickly

**Plateau**: Learning slows down

**Continued Decrease**: Model improving

**Increase**: Possible overfitting

**Noisy/Bumpy**: Normal variation

**Tip**: Use smoothing and log scale to see trends

---

## Slide 23: Common Issues
**Problem**: Loss not decreasing
- **Solution**: More data, adjust learning rate

**Problem**: Validation loss increasing
- **Solution**: Reduce epochs, add regularization

**Problem**: Poor test performance
- **Solution**: Check data quality, try more examples

**Problem**: Inconsistent outputs
- **Solution**: More diverse training examples

---

## Slide 24: Best Practices
1. **Start Small**: 50-100 examples first
2. **Quality > Quantity**: Clean, diverse data
3. **Validate**: Always use validation set
4. **Monitor**: Watch training metrics
5. **Test Thoroughly**: Use held-out test set
6. **Iterate**: Experiment with hyperparameters
7. **Document**: Track what works

---

## Slide 25: Data Quality Matters
**Good Training Data**:
✅ Diverse examples
✅ Consistent format
✅ Accurate labels
✅ Representative of use case

**Bad Training Data**:
❌ Repetitive examples
❌ Inconsistent format
❌ Incorrect labels
❌ Biased samples

---

## Slide 26: Frontier vs. Open Source
**Frontier Model Fine-Tuning**:
- API-based (easy)
- Limited control
- Best for style/format
- Expensive at scale

**Open Source Fine-Tuning** (next topic):
- Full control
- Can teach new knowledge
- Requires more expertise
- Cost-effective at scale

---

## Slide 27: Real-World Example
**Task**: Product price estimation

**Approach**: SFT with product descriptions → prices

**Data**: 20,000 examples

**Result**: Mixed (fine-tuning not ideal for this)

**Lesson**: Fine-tuning frontier models works best for style/format, not core knowledge

---

## Slide 28: Why It Didn't Work
**The Problem**:
- Trying to teach specific domain knowledge
- Frontier model already has general knowledge
- Task better suited for open source fine-tuning

**What Works Better**:
- Prompt engineering with examples
- RAG with product database
- Fine-tuning smaller open source model

---

## Slide 29: Experiments Are Learning
**Data Science Reality**:
- Not all experiments succeed
- Failures eliminate approaches
- Iteration is the process
- R&D requires exploration

**Mindset**: Failed experiments = valuable learning

**Unlike Software**: Bugs are bad, but failed experiments are progress

---

## Slide 30: Alternative: Deep Neural Networks
**When Fine-Tuning Fails**:
- Build custom neural network
- Train from scratch on your data
- Full control over architecture

**Trade-offs**:
- Requires more expertise
- Longer training time
- Can achieve excellent results

**Example**: 289M parameter network → competitive with frontier models

---

## Slide 31: Key Takeaways
1. **Fine-tuning** adapts pre-trained models with small parameter changes
2. **SFT** is most common (labeled data)
3. **Start small** (50-100 examples)
4. **Best for** style, format, edge cases
5. **Not ideal for** teaching new core knowledge
6. **Monitor** loss metrics during training
7. **Iterate** based on results

---

## Slide 32: Resources
**OpenAI Documentation**:
- https://platform.openai.com/docs/guides/fine-tuning
- https://platform.openai.com/finetune (dashboard)

**API Reference**:
- https://platform.openai.com/docs/api-reference/fine-tuning

**Pricing**:
- https://openai.com/api/pricing/

**Community**:
- OpenAI Forum: https://community.openai.com/

---

## Slide 33: Practical Tips
1. **Validate your JSONL** before uploading
2. **Use suffix** parameter for easy identification
3. **Set seed=42** for reproducibility
4. **Monitor in dashboard** for visual feedback
5. **Save model IDs** for later use
6. **Test incrementally** as you iterate
7. **Compare to baseline** (unfine-tuned model)

---

## Slide 34: Next Steps
**Practice**:
- Fine-tune with 20-100 examples
- Experiment with hyperparameters
- Try different use cases

**Explore**:
- DPO for preference learning
- RFT for reasoning tasks
- Open source fine-tuning (more control)

**Apply**:
- Identify your use case
- Prepare quality data
- Iterate and improve

---

## Slide 35: Challenge
**Your Task**:
1. Choose a use case (style/format/edge case)
2. Prepare 50-100 training examples
3. Create validation set
4. Fine-tune GPT-4.1-nano
5. Evaluate results
6. Iterate with different hyperparameters

**Goal**: Understand when fine-tuning helps vs. hurts

---

## Slide 36: Questions to Consider
- When should you fine-tune vs. use RAG?
- How do you know if you have enough data?
- What metrics matter for your use case?
- How do you balance cost vs. performance?
- When is open source fine-tuning better?

---

## Slide 37: Thank You!
**What We Covered**:
- Three types of fine-tuning (SFT, DPO, RFT)
- When fine-tuning works (and when it doesn't)
- The complete fine-tuning workflow
- Monitoring and evaluation
- Best practices and common pitfalls

**Remember**: Fine-tuning is a tool, not always the solution!

**Next**: Open source model fine-tuning for more control

