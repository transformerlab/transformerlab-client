# transformerlab-client

Python client and callbacks for [TransformerLab](https://github.com/transformerlab).

## Install

```bash
pip install transformerlab-client
```

## Prerequisites

- Python 3.10 or newer
- A running instance of the Transformer Lab API server
  - By default, the client connects to `http://localhost:8338/trainer_rpc`
  - Make sure the Transformer Lab application is running before using this client

## Usage

```python
from transformers import TrainingArguments, Trainer
from transformerlab_client.client import TransformerLabClient
from transformerlab_client.callbacks.hf_callback import TLabProgressCallback

# Initialize client and register job
client = TransformerLabClient(server_url=<ENTER YOUR SERVER URL and PORT WHERE THE TRANSFORMER LAB API IS RUNNING>)
job_id = client.start_job(your_config)

# Set up Hugging Face trainer with TLabProgressCallback
training_args = TrainingArguments(
    output_dir="./output",
    # other arguments...
)

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    # other arguments...
    callbacks=[TLabProgressCallback(client)]  # Add TransformerLab callback
)

# Train the model
trainer.train()

# Complete the job
client.complete_job()
```

### Full Training Example

See the examples directory for a complete training script that demonstrates how to use the client for a full training workflow with Hugging Face Transformers.

## API Reference

### TransformerLabClient

Main client for communicating with TransformerLab.

- `start_job(config)`: Register a training job and get a job ID
- `report_progress(progress, metrics=None)`: Report training progress (0-100) and any metrics in json format
- `complete_job(message="Training completed successfully")`: Mark job as complete
- `stop_job(message="Training stopped")`: Mark job as stopped
- `save_model(saved_model_path)`: Save model to specified path
- `log_info(message)`: Log info message
- `log_error(message)`: Log error message
- `log_warning(message)`: Log warning message
- `log_debug(message)`: Log debug message

### TLabProgressCallback

Callback for Hugging Face Transformers Trainer that reports progress to TransformerLab.