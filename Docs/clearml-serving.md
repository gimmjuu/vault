## ClearML-Serving CLI Scripts

### ClearML Setup for Local Access
```
api {
    web_server:http://192.168.0.202:8080
    api_server:http://192.168.0.202:8008
    files_server:http://192.168.0.202:8081
    # serving-test
    credentials {
    "access_key"="IQBGE2PCCFTD9XFLMB5SF60J0RM5K3"
    "secret_key"="qhaBx_v4TGbMDdcD9mDPfA9-FR3fqhFf0xzydrcFUy2MrpRZ1eI-k4n_1XIW9wmTEa8"
    }
}
```
---
### Install clearml-serving CLI
`$ pip3 install clearml-serving`

### Create the Serving Service Controller
`$ clearml-serving create --name "serving example"`
→ New Serving Service created: id=10c97da9513e4727b27fc05b01d8c6fa

### Edit the environment variables file
> CLEARML_WEB_HOST="http://192.168.0.202:8080"
> CLEARML_API_HOST="http://192.168.0.202:8008"
> CLEARML_FILES_HOST="http://192.168.0.202:8081"
> CLEARML_API_ACCESS_KEY="IQBGE2PCCFTD9XFLMB5SF60J0RM5K3"
> CLEARML_API_SECRET_KEY="qhaBx_v4TGbMDdcD9mDPfA9-FR3fqhFf0xzydrcFUy2MrpRZ1eI-k4n_1XIW9wmTEa8"
> CLEARML_SERVING_TASK_ID="10c97da9513e4727b27fc05b01d8c6fa"

### Spin up the clearml-serving containers with docker-compose
`$ cd docker && docker-compose --env-file example.env -f docker-compose-triton-gpu.yml up `

### Register train model
`clearml-serving --id 10c97da9513e4727b27fc05b01d8c6fa model add --engine sklearn --endpoint "test_model_sklearn" --preprocess "examples/sklearn/preprocess.py" --name "train sklearn model - sklearn-model" --project "serving examples" `

### Spin inference container
1. Build container
`$ docker build --tag clearml-serving-inference:latest -f clearml_serving/serving/Dockerfile . `
2. Spin container
`docker run -v ~/clearml.conf:/root/clearml.conf -p 8080:8080 -e CLEARML_SERVING_TASK_ID=10c97da9513e4727b27fc05b01d8c6fa -e CLEARML_SERVING_POLL_FREQ=5 clearml-serving-inference:latest `
3. Test inference endpoint
`curl -X POST "http://127.0.0.1:8080/serve/test_model_sklearn" -H "accept: application/json" -H "Content-Type: application/json" -d '{"x0": 1, "x1": 2}' `

### Auto Deployment
`clearml-serving --id 10c97da9513e4727b27fc05b01d8c6fa model auto-update --engine sklearn --endpoint "test_model_sklearn_auto" --preprocess "examples/sklearn/preprocess.py" --name "train sklearn model auto" --project "serving examples" --max-versions 2` `

### Monitoring
`clearml-serving --id 10c97da9513e4727b27fc05b01d8c6fa metrics add --endpoint test_model_sklearn --variable-scalar x0=0,0.1,0.5,1,10 x1=0,0.1,0.5,1,10 y=0,0.1,0.5,0.75,1 `

### Manually model registration
`clearml-serving --id 10c97da9513e4727b27fc05b01d8c6fa model upload --name "manual yolov8 model" --project "serving examples" --framework "onnx" --path ./best.pt `
→ Model created and registered, new Model ID=febe809cdb90499f8982adf636fb547f

> [cf] Usable framework
> tensorflow, tensorflowjs, tensorflowlite, pytorch, torchscript, caffe, caffe2, onnx, keras, mknet, cntk, torch, darknet, paddlepaddle, scikitlearn, xgboost, lightgbm, parquet, megengine, catboost, tensorrt, openvino, custom

#### Add new endpoint
`clearml-serving --id 10c97da9513e4727b27fc05b01d8c6fa model add --engine sklearn --endpoint "test_model_yolov8" --preprocess "examples/sklearn/preprocess.py" --model-id febe809cdb90499f8982adf636fb547f `
