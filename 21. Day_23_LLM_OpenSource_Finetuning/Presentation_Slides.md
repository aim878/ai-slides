# Fine-Tuning Open-Source LLMs with QLoRA
## Presentation Slides

---

## Slide 1: Title Slide
**Fine-Tuning Open-Source LLMs with QLoRA**

Building Your Own Specialized Language Model

---

## Slide 2: What Does "Building Your Own Model" Mean?

**Reality Check:**
- We're NOT training from scratch
- We're NOT building a new architecture
- We ARE fine-tuning an existing open-source model

**Common Base Models:**
- Meta's Llama 3.2 (most popular in industry)
- Google's Gemma
- Microsoft's Phi
- DeepSeek
- Alibaba's Qwen

---

## Slide 3: Why Fine-Tune Open-Source Models?

**Advantages:**
- ✅ Free to use and deploy
- ✅ Run on your own hardware
- ✅ Full control over the model
- ✅ No API costs
- ✅ Data privacy (runs locally)

**Challenge:**
- Smaller models (3B parameters) vs Frontier models (100B+ parameters)
- Need efficient training techniques

---

## Slide 4: The Challenge

**Problem:**
- Llama 3.2 (3B parameters) = 13GB of memory
- Training requires 4x more memory (forward pass, backward pass, gradients, optimizer states)
- **Total: ~50GB+ GPU memory needed!**

**Solution:** QLoRA

---

## Slide 5: What is QLoRA?

**QLoRA = Quantization + LoRA**

Two separate tricks combined:
1. **LoRA** (Low-Rank Adaptation)
2. **Q** (Quantization)

Both techniques reduce memory requirements dramatically!

---

## Slide 6: LoRA - The Core Idea

**Problem:** 3 billion parameters are too many to train

**LoRA Solution:**
1. **Freeze** all original model weights (don't train them!)
2. Add small **adapter matrices** (LoRA matrices)
3. Train only these small matrices
4. Add them to the frozen base model

**Result:** Train 18 million parameters instead of 3 billion!

---

## Slide 7: LoRA - How It Works

**Architecture:**
- Base Model: Llama 3.2 (frozen, 3B parameters)
- Target Modules: Attention layers (Q, K, V, O)
- LoRA Matrices: Two small matrices (A and B)

**Formula:**
```
Output = Base_Layer + (alpha × LoRA_A × LoRA_B)
```

**Key Point:** LoRA_A and LoRA_B are much smaller than base layers!

---

## Slide 8: LoRA Hyperparameters

**r (rank):** Number of dimensions in LoRA matrices
- Common values: 8, 16, 32, 64
- Higher r = more capacity, slower training

**alpha:** Scaling factor
- Rule of thumb: alpha = 2 × r
- Example: r=32 → alpha=64

**target_modules:** Which layers to adapt
- Start with: Attention layers (q_proj, k_proj, v_proj, o_proj)
- Advanced: Add MLP layers for more capacity

---

## Slide 9: Quantization - The Q in QLoRA

**What is Quantization?**
- Reduce precision of each weight
- 32-bit float → 8-bit → 4-bit

**Analogy:** Like a dimmer switch
- 32-bit: Infinite positions (smooth)
- 8-bit: 256 positions (fine)
- 4-bit: 16 positions (coarse)

**Trade-off:**
- ✅ Massive memory savings (13GB → 2.2GB)
- ⚠️ Slight performance drop (but not much!)

---

## Slide 10: Memory Comparison

| Configuration | Memory Size |
|---------------|-------------|
| Llama 3.2 (32-bit) | 13 GB |
| Llama 3.2 (8-bit) | 3.6 GB |
| Llama 3.2 (4-bit) | 2.2 GB |
| + LoRA (r=32) | +73 MB |
| **Total QLoRA** | **2.27 GB** |

**You can fine-tune on a free Google Colab T4 GPU!**

---

## Slide 11: Base Model vs Chat Model

**Two Types of Models:**

**Base Model:**
- Trained to predict next token
- Simple: Input → Completion
- Best for: Specialized tasks with one instruction type

**Chat/Instruct Model:**
- Fine-tuned for conversations
- Expects: System prompt, User message, Assistant response
- Best for: Interactive applications with varied instructions

**For QLoRA:** Choose based on your task!

---

## Slide 12: Training Hyperparameters

**Epochs:** Number of complete passes through data
- Start with: 1-3 epochs
- More epochs = more training, risk of overfitting

**Batch Size:** How many examples per training step
- Maximize based on GPU memory
- Common: 4, 8, 16

**Learning Rate:** How big each training step is
- Typical: 1e-4 to 5e-4
- Too high = unstable, too low = slow

---

## Slide 13: Dropout

**What is Dropout?**
- Randomly "turn off" 10-20% of neurons during training
- Different neurons each time

**Why?**
- Prevents overfitting
- Forces model to learn general patterns
- Makes model more robust

**Setting:** Usually 0.1 (10%) or 0.2 (20%)

---

## Slide 14: Data Preparation

**Format:** Prompt + Completion pairs

**Example:**
```
Prompt: "What does this cost to the nearest dollar? 
         [product description]
         Price is $"

Completion: "149"
```

**Key Points:**
- Keep prompts consistent
- Limit token length (e.g., 110 tokens)
- Round outputs if needed (easier to learn)

---

## Slide 15: The Training Process

**Steps:**
1. Load base model (quantized to 4-bit)
2. Add LoRA adapters
3. Prepare data (prompt-completion pairs)
4. Configure trainer (SFTTrainer)
5. Train! (monitor loss)
6. Save LoRA weights
7. Evaluate on test set

**Time:** 30 minutes to several hours depending on data size

---

## Slide 16: Monitoring Training

**Key Metrics:**
- **Training Loss:** Should decrease over time
- **Validation Loss:** Should decrease (watch for overfitting)
- **GPU Memory:** Should stay under limit

**Tools:**
- TensorBoard for visualization
- Weights & Biases (wandb) for tracking
- Hugging Face Hub for model storage

---

## Slide 17: When to Use QLoRA Fine-Tuning

**Good Use Cases:**
- ✅ Specialized domain knowledge
- ✅ Specific output format
- ✅ Custom tone/style
- ✅ Privacy-sensitive data
- ✅ Cost optimization (no API fees)

**Not Ideal For:**
- ❌ General knowledge (use frontier models)
- ❌ Very small datasets (<100 examples)
- ❌ Tasks that need reasoning (use o1/o3)

---

## Slide 18: Practical Considerations

**Hardware:**
- Google Colab (free T4): 15GB GPU RAM
- Local GPU: RTX 3090/4090 recommended
- Cloud: AWS, GCP, Azure with GPU instances

**Costs:**
- Free: Google Colab (with limits)
- Paid: ~$0.50-2.00/hour for cloud GPUs

**Time Investment:**
- Data preparation: Hours to days
- Training: 30 min - 4 hours
- Evaluation: Minutes

---

## Slide 19: Best Practices

1. **Start Small:** Use r=8 or r=16 first
2. **Monitor Overfitting:** Watch validation loss
3. **Experiment:** Try different hyperparameters
4. **Version Control:** Save models at checkpoints
5. **Document Everything:** Track what works
6. **Test Thoroughly:** Evaluate on held-out data

---

## Slide 20: Common Pitfalls

**Avoid These Mistakes:**
- ❌ Not enough training data (need 1000+ examples ideally)
- ❌ Too many epochs (overfitting)
- ❌ Wrong base model choice (base vs instruct)
- ❌ Ignoring data quality
- ❌ Not setting max token length
- ❌ Forgetting to quantize base model

---

## Slide 21: Tools & Libraries

**Essential:**
- `transformers` - Hugging Face library
- `peft` - Parameter-Efficient Fine-Tuning
- `bitsandbytes` - Quantization
- `trl` - Transformer Reinforcement Learning (SFTTrainer)

**Optional:**
- `wandb` - Experiment tracking
- `tensorboard` - Visualization
- `datasets` - Data loading

---

## Slide 22: Deployment Options

**After Training:**
1. **Local Deployment:** Run on your machine
2. **Cloud API:** Deploy on AWS/GCP/Azure
3. **Edge Devices:** Mobile phones, IoT (for small models)
4. **Hugging Face Spaces:** Free hosting for demos

**Inference:**
- Load base model + LoRA weights
- Much faster than training!
- Can quantize further for deployment

---

## Slide 23: Real-World Example

**Task:** Predict product prices from descriptions

**Setup:**
- Base: Llama 3.2 (3B, 4-bit)
- Data: 20,000 product descriptions
- LoRA: r=32, alpha=64
- Training: 1 epoch, ~30 minutes

**Results:**
- Base model: MAE = $110
- After fine-tuning: MAE = $30-40
- **73% improvement!**

---

## Slide 24: Resources

**Documentation:**
- Hugging Face: https://huggingface.co/docs/peft
- QLoRA Paper: https://arxiv.org/abs/2305.14314
- LoRA Paper: https://arxiv.org/abs/2106.09685

**Tutorials:**
- Hugging Face Course: https://huggingface.co/learn
- Google Colab Examples: Search "QLoRA fine-tuning"

**Community:**
- Hugging Face Forums
- Reddit: r/LocalLLaMA
- Discord: Hugging Face, EleutherAI

---

## Slide 25: Key Takeaways

1. **QLoRA = Quantization + LoRA** (two memory-saving tricks)
2. **Train 18M parameters** instead of 3B
3. **Use 2.2GB memory** instead of 50GB+
4. **Fine-tune on free hardware** (Google Colab)
5. **Choose base vs instruct** model based on task
6. **Start with r=32, alpha=64** for attention layers
7. **Monitor training loss** and validation loss
8. **Experiment with hyperparameters** to optimize

---

## Slide 26: Next Steps

**To Get Started:**
1. Set up Google Colab with T4 GPU
2. Install required libraries
3. Load Llama 3.2 (or similar model)
4. Prepare your dataset
5. Configure QLoRA parameters
6. Train and evaluate!

**Remember:** Fine-tuning is iterative - experiment and improve!

---

## Slide 27: Questions?

**Thank you!**

Ready to build your own specialized LLM?

---

## Additional Resources Slide

**Useful Links:**
- Llama Models: https://huggingface.co/meta-llama
- PEFT Library: https://github.com/huggingface/peft
- TRL Library: https://github.com/huggingface/trl
- Bits and Bytes: https://github.com/TimDettmers/bitsandbytes
- Example Notebooks: https://github.com/huggingface/peft/tree/main/examples

---

**End of Presentation**

