---
language:
- en
license: other
license_name: veda-labs-license
tags:
- image-generation
- image-editing
- vedika
- diffusion-single-file
pipeline_tag: text-to-image
library_name: diffusers
---

![Vedika Amazing 2.0 Banner](./778972803_2325215891583108_6267115065853050695_n.webp)

# Vedika Amazing 2.0

**Vedika Amazing 2.0** is a state-of-the-art Diffusers-based pipeline for text-to-image generation and image editing. Built on advanced transformer architecture, Vedika Amazing 2.0 delivers exceptional quality and creative control for developers and artists.

For more information, visit our official website: [https://vedalabs.online](https://vedalabs.online)

Follow us on Twitter/X: [@VedaLabsAI](https://twitter.com/VedaLabsAI)

## Key Features

1. **State-of-the-art Generation**: Advanced text-to-image capabilities with high-quality output
2. **Reference-Based Editing**: Single and multi-reference image editing without additional fine-tuning
3. **Efficient Inference**: Optimized for consumer-grade GPUs with quantization support
4. **Open Weights**: Available for research, development, and creative workflows
5. **Flexible Licensing**: Suitable for personal, educational, and commercial use (with appropriate license)

## Repository Structure

```
vedika-amazing-2.0/
├── model_index.json          # Pipeline configuration
├── vedika_pipeline.py        # Custom pipeline implementation
├── README.md                 # This file
├── LICENSE.md                # Veda Labs License
├── scheduler/
│   └── scheduler_config.json
├── text_encoder/
│   └── config.json
├── tokenizer/
│   ├── tokenizer_config.json
│   ├── tokenizer.json
│   ├── special_tokens_map.json
│   ├── processor_config.json
│   └── chat_template.json
├── transformer/
│   └── config.json
└── vae/
    └── config.json
```

**Note**: Large weight files (*.safetensors) are hosted on Hugging Face and will be automatically downloaded when using the pipeline.

## Usage

### Using with Diffusers 🧨

Vedika Amazing 2.0 is compatible with the Hugging Face Diffusers library. Here's how to get started:

```python
import torch
from diffusers import VedikaPipeline

# Load the pipeline with automatic weight streaming from Hugging Face
pipe = VedikaPipeline.from_pretrained(
    "VedaLabsAI/vedika-amazing-2.0",
    torch_dtype=torch.bfloat16,
    device_map="auto"  # Automatically distribute components across available devices
)

# Generate an image from a text prompt
prompt = "A beautiful sunset over mountains, photorealistic, 8k"
image = pipe(
    prompt=prompt,
    num_inference_steps=50,
    guidance_scale=4.0,
    generator=torch.Generator(device="cuda").manual_seed(42)
).images[0]

# Save the generated image
image.save("vedika_output.png")
```

### Advanced Usage with Remote Text Encoder

For memory-constrained environments, you can use a remote text encoder:

```python
import torch
from diffusers import VedikaPipeline
from huggingface_hub import get_token
import requests
import io

def remote_text_encoder(prompts):
    """Fetch text embeddings from a remote endpoint."""
    response = requests.post(
        "https://remote-text-encoder.vedalabs.online/predict",
        json={"prompt": prompts},
        headers={
            "Authorization": f"Bearer {get_token()}",
            "Content-Type": "application/json"
        }
    )
    prompt_embeds = torch.load(io.BytesIO(response.content))
    return prompt_embeds.to("cuda")

# Initialize pipeline without text encoder
pipe = VedikaPipeline.from_pretrained(
    "VedaLabsAI/vedika-amazing-2.0",
    text_encoder=None,
    torch_dtype=torch.bfloat16
).to("cuda")

prompt = "Realistic macro photograph of a hermit crab using a soda can as its shell"

# Generate using remote text embeddings
image = pipe(
    prompt_embeds=remote_text_encoder(prompt),
    generator=torch.Generator(device="cuda").manual_seed(42),
    num_inference_steps=50,
    guidance_scale=4.0,
).images[0]

image.save("vedika_output.png")
```

### Quantized Model for Consumer GPUs

For deployment on consumer graphics cards like RTX 4090 or RTX 5090:

```python
import torch
from diffusers import VedikaPipeline

# Load quantized version for efficient inference
pipe = VedikaPipeline.from_pretrained(
    "VedaLabsAI/vedika-amazing-2.0-bnb-4bit",
    torch_dtype=torch.bfloat16
).to("cuda")

prompt = "A cat holding a sign that says hello world"
image = pipe(
    prompt=prompt,
    num_inference_steps=28,  # 28 steps provides good quality/speed trade-off
    guidance_scale=4.0,
).images[0]

image.save("vedika_quick.png")
```

## Installation Requirements

```bash
pip install diffusers transformers accelerate torch torchvision
```

For quantized inference:
```bash
pip install bitsandbytes
```

## Model Specifications

- **Architecture**: Rectified Flow Transformer (MMDiT)
- **Parameters**: Optimized for efficient inference
- **Precision**: bfloat16 native support
- **Scheduler**: FlowMatchEulerDiscreteScheduler
- **Text Encoder**: Mistral3-based multimodal encoder
- **VAE**: AutoencoderKLFlux2 with patch-based compression

## License

This project is licensed under the **Veda Labs License**. See the [LICENSE.md](./LICENSE.md) file for complete terms and conditions.

**Important**: 
- Non-commercial use is permitted under this license
- Commercial use requires a separate commercial license from Veda Labs
- Proper attribution must be provided when distributing the Model or derivatives
- Generated outputs may be used commercially subject to license compliance

For commercial licensing inquiries, please contact: **licensing@vedalabs.online**

## Citation

If you use Vedika Amazing 2.0 in your research or projects, please cite:

```bibtex
@software{vedika_amazing_2_0,
  title = {Vedika Amazing 2.0},
  author = {Veda Labs},
  year = {2025},
  url = {https://github.com/VedaLabsAI/vedika-amazing-2.0}
}
```

## Contact & Support

- **Website**: [https://vedalabs.online](https://vedalabs.online)
- **Twitter/X**: [@VedaLabsAI](https://twitter.com/VedaLabsAI)
- **Email**: support@vedalabs.online
- **License Questions**: legal@vedalabs.online

---

© 2025 Veda Labs. All rights reserved. | [License](./LICENSE.md) | [Privacy Policy](https://vedalabs.online/privacy) | [Terms of Service](https://vedalabs.online/terms)
