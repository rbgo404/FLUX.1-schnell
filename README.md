# Model Template - FLUX.1-schnell
[FLUX.1-schnell](https://huggingface.co/black-forest-labs/FLUX.1-schnell) is an open-source image generation model from the FLUX.1 suite of text-to-image models that define a new state-of-the-art in image detail, prompt adherence, style diversity and scene complexity for text-to-image synthesis. FLUX.1 suite developed by Black Forest Labs. It is a 12 billion parameter rectified flow transformer capable of generating images from text descriptions.

### Fork the Repository
Get started by forking the repository. You can do this by clicking on the fork button in the top right corner of the repository page.

This will create a copy of the repository in your own GitHub account, allowing you to make changes and customize it according to your needs.

### Create a Custom Runtime in Inferless
To access the custom runtime window in Inferless, simply navigate to the sidebar and click on the Create new Runtime button. A pop-up will appear.

Next, provide a suitable name for your custom runtime and proceed by uploading the **inferless-runtime-config.yaml** file given above. Finally, ensure you save your changes by clicking on the save button.

### Import the Model in Inferless
Log in to your inferless account, select the workspace you want the model to be imported into and click the Add Model button.

Select the PyTorch as framework and choose **Repo(custom code)** as your model source and select your provider, and use the forked repo URL as the **Model URL**.

Enter all the required details to Import your model. Refer [this link](https://docs.inferless.com/integrations/git-custom-code/git--custom-code) for more information on model import.

---
## Curl Command
Following is an example of the curl command you can use to make inference. You can find the exact curl command in the Model's API page in Inferless.
```bash
curl --location '<your_inference_url>' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer <your_api_key>' \
    --data '{
              {
                "inputs": [
                  {
                    "name": "prompt",
                    "shape": [1],
                    "data": ["A cat holding a sign that says hello"],
                    "datatype": "BYTES"
                  },
                  {
                    "name": "height",
                    "optional": true,
                    "shape": [1],
                    "data": [512],
                    "datatype": "INT16"
                  },
                  {
                    "name": "width",
                    "optional": true,
                    "shape": [1],
                    "data": [512],
                    "datatype": "INT16"
                  },
                  {
                    "name": "num_inference_steps",
                    "optional": true,
                    "shape": [1],
                    "data": [50],
                    "datatype": "INT16"
                  },
                  {
                    "name": "guidance_scale",
                    "optional": true,
                    "shape": [1],
                    "data": [7.5],
                    "datatype": "FP32"
                  },
                  {
                    "name": "max_sequence_length",
                    "optional": true,
                    "shape": [1],
                    "data": [256],
                    "datatype": "INT16"
                  }
                ]
              }

    }'
```

---
## Customizing the Code
Open the `app.py` file. This contains the main code for inference. It has three main functions, initialize, infer and finalize.

**Initialize** -  This function is executed during the cold start and is used to initialize the model. If you have any custom configurations or settings that need to be applied during the initialization, make sure to add them in this function.

**Infer** - This function is where the inference happens. The argument to this function `inputs`, is a dictionary containing all the input parameters. The keys are the same as the name given in inputs. Refer to [input](https://docs.inferless.com/model-import/input-output-schema) for more.

```python
def infer(self, inputs):
    prompt = inputs["prompt"]
    height = inputs.get("height", 512)
    width = inputs.get("width", 512)
    guidance_scale = inputs.get("guidance_scale", 7.5)
    num_inference_steps = inputs.get("num_inference_steps", 50)
    max_sequence_length = inputs.get("max_sequence_length", 256)
```

**Finalize** - This function is used to perform any cleanup activity for example you can unload the model from the gpu by setting to `None`.
```python
def finalize(self):
    self.pipe = None
```


For more information refer to the [Inferless docs](https://docs.inferless.com/).
