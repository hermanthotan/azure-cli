The purpose of this project is to getting to know the Azure Media Services.
How to build streaming website like netflix or youtube.

# Create Media Services account

## Login
```sh
az login
az account list
```
## Create a resource group
```sh
az group create -n amsrg -l southeastasia
```
or
```sh
az group create -n amsrg -l southeastasia --subcription {id}
```

## Create an Azure storage account
```sh
az storage account create -n amssa001 --kind StorageV2 --sku Standard_LRS -l southeastasia -g amsrg
```

## Create an Azure Media Services account
```sh
az ams account create --n amsacc -g amsrg --storage-account amssa001 -l southeastasia

```

## Start the streaming endpoint
```sh
az ams streaming endpoint start -n default -a amsacc -g amsrg
```

## Create a transform for adaptive bitrate encoding
```sh
az ams transform create -n encodingadaptive --preset-names AdaptiveStreaming --description "Encoding adaptive bitrate encoding" -g amsrg -a amsacc
```

## Create an output asset
```sh
az ams asset create -n output -a amsacc -g amsrg
```

## Start a job by using HTTPS input
```sh
az ams job start -g amsrg -a amsacc --name job001 --transform-name encodingadaptive --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/' --files 'Ignite-short.mp4' --output-asset-names 'output'
```

## Check status
```sh
az ams job show -g amsrg -a amsacc -t encodingadaptive -n job001 -o table
```

# Create a streaming locator and get a path

## Create a streaming locator
```sh
az ams streaming locator create -g amsrg -a amsacc -n streamlocator --asset-name output --streaming-policy-name Predefined_ClearStreamingOnly
```

## Get streaming locator paths
```sh
az ams streaming locator get-paths -g amsrg -a amsacc -n streamlocator
```

