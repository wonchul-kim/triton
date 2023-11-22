# TRITON

## SERVER

#### 1. Prepare the docker image for `Driver Version: 470.223.02`
```cmd
docker pull nvcr.io/nvidia/tritonserver:21.02-py3
```

#### 2. Deploy

- Use examples provided by NVIDIA

```cmd
git clone git@github.com:triton-inference-server/server.git
cd server/docs/examples
./fetch_models.sh

docker run --gpus 1 --rm -p 9000:8000 -p 9001:8001 -p 9002:8002 -v $(pwd)/model_repository:/models nvcr.io/nvidia/tritonserver:21.02-py3 tritonserver --model-repository=/models
```

- Or, we can use our models. It just requires to change the `model_repository` directory after building like the below:

```
model_respository/
|--- <model_name>/
|    |--- <version>/
|    |    |--- <weights>
|    |--- config.pbtxt    
|    |--- <model_name>_labels.txt
|--- <model_name>/
|    |--- <version>/
|    |    |--- <weights>
|    |--- config.pbtxt    
|    |--- <model_name>_labels.txt
|--- deeplabv3plus/
     |--- 1/
     |    |--- deeplabv3plus_best.pt
     |--- config.pbtxt    
     |--- deeplabv3plus_labels.txt
```

- config

    - plaform: 
        - TensorFlow:`tensorflow`
        - PyTorch: `pytorch`
        - ONNX (Open Neural Network Exchange): `onnx`
        - TensorRT (NVIDIA TensorRT): `tensorrt`
        - TensorRT PLAN (Serialized TensorRT plans): `tensorrt_plan`
        - TensorRT PLAN (Serialized TensorRT plans with dynamic shapes): `tensorrt_plan_dynamic`
        - Custom Frameworks (using an external custom backend): `custom`


#### 3. Verify triton server is running
```cmd
curl -v localhost:9000/v2/health/ready
```

-----------------------------------------------------

## Client

#### 1. Install `tritonclient`
```cmd
sudo apt update
sudo apt install libb64-dev
pip install "tritonclient[http]"
```

#### 2. Infer the model 
```cmd
python infer.py
```



## References
- [NVIDIA TRITON SERVER Image](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tritonserver/tags)
