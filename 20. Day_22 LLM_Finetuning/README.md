# Fine-Tuning Frontier LLMs

Welcome to the Fine-Tuning Frontier LLMs tutorial! This folder contains comprehensive materials for learning how to fine-tune state-of-the-art language models using OpenAI's API.

## 📁 Folder Contents

- **Presentation_Slides.md** - 37 slides covering fine-tuning concepts and best practices
- **Lecture_Notes.md** - Comprehensive educational content with detailed explanations
- **Finetuning_Frontier_LLMs.ipynb** - Step-by-step Jupyter notebook with complete workflow
- **requirements.txt** - Python dependencies
- **README.md** - This file (setup instructions)

## 🎯 What You'll Learn

### Core Concepts
1. **LoRA (Low Rank Adapters)** - How fine-tuning works under the hood
2. **Three Types of Fine-Tuning** - SFT, DPO, and RFT
3. **Complete Workflow** - From data preparation to deployment
4. **Best Practices** - When to use fine-tuning (and when not to)
5. **Evaluation** - How to measure and improve results

### Key Topics
- JSONL data format preparation
- Uploading files to OpenAI
- Creating and monitoring fine-tuning jobs
- Testing and comparing models
- Understanding loss metrics
- Hyperparameter tuning
- Cost optimization

## 🚀 Setup Instructions

### Step 1: Create a Conda Environment

```bash
# Create a new conda environment
conda create -n finetuning-env python=3.11 -y

# Activate the environment
conda activate finetuning-env
```

### Step 2: Install Dependencies

```bash
# Navigate to the LLM_Finetuning folder
cd LLM_Finetuning

# Install required packages
pip install -r requirements.txt
```

### Step 3: Set Up API Keys

Create a `.env` file in the LLM_Finetuning folder:

```bash
# On Windows
type nul > .env

# On Mac/Linux
touch .env
```

Add your OpenAI API key to the `.env` file:

```
OPENAI_API_KEY=your_openai_api_key_here
```

**Get your API key**: https://platform.openai.com/api-keys

### Step 4: Run the Notebook

```bash
# Launch Jupyter
jupyter notebook Finetuning_Frontier_LLMs.ipynb
```

Or use VS Code / Cursor with the Jupyter extension.

## 📊 What's in the Notebook

### Complete Workflow

1. **Setup and Imports** - Initialize OpenAI client
2. **Prepare Training Data** - Create JSONL format examples
3. **Upload Files** - Send data to OpenAI
4. **Create Fine-Tuning Job** - Configure and start training
5. **Monitor Progress** - Watch training metrics
6. **Test Model** - Compare fine-tuned vs. base model
7. **Evaluate Results** - Analyze performance

### Example Use Case

The notebook demonstrates fine-tuning a model to respond in a specific style (helpful, concise answers to technical questions).

## 🔑 Key Concepts

### When Fine-Tuning Works

✅ **Good Use Cases**:
- Setting style or tone
- Ensuring consistent output format
- Correcting specific failures
- Handling edge cases
- Tasks hard to explain in prompts

❌ **Poor Use Cases**:
- Teaching completely new knowledge
- When prompt engineering works well
- Tasks requiring massive domain expertise
- When you have very limited examples

### Types of Fine-Tuning

| Type | Description | Best For |
|------|-------------|----------|
| **SFT** | Supervised Fine-Tuning | Labeled input-output pairs |
| **DPO** | Direct Preference Optimization | Good vs. bad examples |
| **RFT** | Reinforcement Fine-Tuning | Graded outputs (LLM as judge) |

**Most Common**: SFT (Supervised Fine-Tuning) - used in the notebook

## 💰 Cost Considerations

**Training Costs** (approximate):
- **GPT-4o-mini**: ~$0.008 per 1K tokens
- **100 examples**: ~$0.10
- **1,000 examples**: ~$1.00
- **10,000 examples**: ~$10.00

**Inference Costs**:
- Slightly higher than base model
- Worth it if fine-tuning provides value

**Recommendation**: Start with 50-100 examples to test

## 📈 Training Process

### Data Format (JSONL)

```json
{"messages": [{"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
{"messages": [{"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
```

### Hyperparameters

- **n_epochs**: Number of passes through data (usually 1)
- **batch_size**: Examples per step (1 for small datasets, 8-16 for large)
- **learning_rate**: Auto-selected by OpenAI (usually best)
- **seed**: For reproducibility (42 is traditional)

### Monitoring

Watch for:
- **Training Loss**: Should decrease over time
- **Validation Loss**: Should not increase (indicates overfitting)
- **Initial Drop**: Normal - model learns format quickly
- **Plateau**: May indicate need for more data or different hyperparameters

## 🔧 Troubleshooting

### Issue: API Key Not Found

**Problem**: `OpenAIError: The api_key client option must be set`

**Solution**:
1. Verify `.env` file exists in LLM_Finetuning folder
2. Check API key format: `OPENAI_API_KEY=sk-...`
3. Restart Jupyter kernel after adding `.env`

### Issue: Training Failed

**Problem**: Fine-tuning job status shows "failed"

**Solution**:
1. Check events: `client.fine_tuning.jobs.list_events(job_id)`
2. Verify data format (valid JSONL)
3. Ensure no policy violations in training data
4. Check file upload status

### Issue: Poor Results

**Problem**: Fine-tuned model not better than base model

**Solution**:
1. Verify you're testing correctly (no assistant response in test input!)
2. Check if task is suitable for fine-tuning
3. Try more diverse training examples
4. Consider if prompt engineering would work better

### Issue: Model Not Available

**Problem**: Model name not recognized

**Solution**:
- Check current supported models: https://platform.openai.com/docs/guides/fine-tuning
- Update model name in notebook (may have changed since tutorial creation)
- Use `gpt-4o-mini-2024-07-18` or latest available

## 📚 Additional Resources

### Official Documentation
- **Fine-Tuning Guide**: https://platform.openai.com/docs/guides/fine-tuning
- **API Reference**: https://platform.openai.com/docs/api-reference/fine-tuning
- **Dashboard**: https://platform.openai.com/finetune
- **Pricing**: https://openai.com/api/pricing/

### Research Papers
- **LoRA**: https://arxiv.org/abs/2106.09685
- **DPO**: https://arxiv.org/abs/2305.18290

### Community
- **OpenAI Forum**: https://community.openai.com/
- **GitHub Discussions**: https://github.com/openai/openai-python/discussions

## 🎓 Learning Path

### Beginner
1. Read the lecture notes
2. Review the slides
3. Run the notebook with example data
4. Experiment with different prompts

### Intermediate
1. Prepare your own training data (50-100 examples)
2. Fine-tune for your specific use case
3. Compare results with base model
4. Iterate with different hyperparameters

### Advanced
1. Try DPO or RFT for advanced scenarios
2. Optimize for production (cost, latency)
3. Implement evaluation metrics
4. Explore open source fine-tuning for more control

## 💡 Best Practices

### Data Preparation
1. **Quality > Quantity**: 50 perfect examples > 500 mediocre ones
2. **Diversity**: Cover different scenarios and edge cases
3. **Consistency**: Maintain same format throughout
4. **Validation Split**: Always hold out validation data

### Training
1. **Start Small**: Begin with 50-100 examples
2. **Monitor Closely**: Watch both training and validation loss
3. **Be Patient**: Don't stop at first loss drop
4. **Document**: Record hyperparameters and results

### Evaluation
1. **Compare to Baseline**: Always test base model too
2. **Multiple Metrics**: Don't rely on single number
3. **Edge Cases**: Test unusual inputs
4. **Human Review**: Quantitative metrics aren't everything

## ⚠️ Important Notes

### What Fine-Tuning IS Good For
- Adjusting style and tone
- Ensuring output format consistency
- Correcting specific, consistent failures
- Handling known edge cases

### What Fine-Tuning IS NOT Good For
- Teaching completely new knowledge (use RAG instead)
- When good prompts already work
- Tasks requiring deep domain expertise (consider open source fine-tuning)
- When you have fewer than 20-30 examples

### Cost Management
- Start with smallest dataset that shows improvement
- Use cheapest model (gpt-4o-mini or nano) for testing
- Monitor costs in OpenAI dashboard
- Delete old fine-tuned models you're not using

## 🚀 Next Steps

After completing this tutorial:

1. **Practice**: Fine-tune with your own use case
2. **Experiment**: Try different hyperparameters
3. **Compare**: Test against prompt engineering and RAG
4. **Learn More**: Explore open source fine-tuning for more control
5. **Deploy**: Integrate fine-tuned model into your application

## 📝 Notes

- **Python Version**: Requires Python 3.11+
- **API Costs**: Monitor usage at https://platform.openai.com/usage
- **Training Time**: Small datasets (5-15 minutes), large datasets (1-2 hours)
- **Model Retention**: Fine-tuned models stored for 3 months (check current policy)

## 🤝 Need Help?

If you encounter issues:

1. Check the Troubleshooting section above
2. Review the lecture notes for concept clarification
3. Verify your API key and environment setup
4. Check OpenAI status: https://status.openai.com/
5. Consult OpenAI documentation and community forum

---

**Happy Fine-Tuning! 🎉**

Remember: Fine-tuning is a powerful tool, but it's not always the right solution. Always consider simpler alternatives (prompt engineering, RAG) first, and use fine-tuning when it truly adds value.

