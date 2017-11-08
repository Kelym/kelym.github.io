---
layout: post
title: Set Up Macbook with GTX1080Ti and Tensorflow
published: true
date: 2017-09-27 10:11:11.000000000 +09:00
categories: misc
tags: tensorflow ml gpu setup
---

**TL;DR** 2017 Macbook Pro: connect to GTX 1080 Ti graphic card, install CUDA and CUDNN, build tensorflow 1.3 with gpu support. 



## Introduction

As I got more involved with large scale deep learning, I decided to install ML libraries with GPU support. Unfortunately, the available documentation for supporting GPU on a Macbook Pro is limited and there is no official solution to even connect Nvidia GPU to Macbook. Thanks to several the online posts, I successfully installed tensorflow on my machine. There were a few issues raised because of versions of libraries involved. So I would clarify as much as possible on those. 


## Spec

**Equipments** I had:

- 2017 MacBook Pro, 13-inch without touch bar. OS build number `16F2073` 
- GPU card: MSI GTX 1080 Ti. 
- eGPU closure: Akitio Node Thunderbolt 3 External, [Bought from BH Photo Video](https://www.bhphotovideo.com/c/product/1303819-REG/akitio_ak_node_t3ia_aktu_thunderbolt3_external_pcie_box.html).

**Environment** I used:

- `homebrew 1.3.4`
- `anaconda + python 3.6`
- I created a env in anaconda use `conda create --name [env_name] python=3.5 numpy scipy matplotlib theano keras ipython jupyter` and `pip 9.0.1` comes with py35. 


## Resources

I relied on these great tutorials to set things up. 

- [eGPU Set up on 2017 15inch MacBook Pro, GTX 1080 Ti + Akitio Node, MacOS](https://egpu.io/forums/implementation-guides/2017-15-macbook-pro-touchbar-gtx1080ti40gbs-tb3akitio-node-macos/)
- [Complete Guide on Macbook + tf 1.3 + GPU](https://metakermit.com/2017/compiling-tensorflow-with-gpu-support-on-a-macbook-pro/)
- [Stack Exchange Outline of Setting Up Macbook + Akitio + 1080 Ti + Tensorflow](https://apple.stackexchange.com/questions/277356/machine-learning-on-external-gpu-with-cuda-and-late-mbp-2016/283903#283903)
- [Complete Guide that setting up MacOS + tensorflow 1.2 + GPU](https://medium.com/@mattias.arro/installing-tensorflow-1-2-from-sources-with-gpu-support-on-macos-4f2c5cab8186)


## Bash Profile

Throughout the process I added a bunch of paths to my bash profile. Here is a summary of them: 

```bash
# =================================
# Set up eGPU driver + tensorflow
# =================================
#
# Step 1.
# CUDA INSTALL
# so that all the CUDA binaries are available to you on the command line:
# 
export PATH=/Developer/NVIDIA/CUDA-8.0/bin${PATH:+:${PATH}}
export DYLD_LIBRARY_PATH=/Developer/NVIDIA/CUDA-8.0/lib${DYLD_LIBRARY_PATH:+:${DYLD_LIBRARY_PATH}}
#
# Step 2.
# CUDNN Installation
#
export DYLD_LIBRARY_PATH="/usr/local/cuda/lib":$DYLD_LIBRARY_PATH

#
# Step 3. 
# Set up env to build tensorflow
# copied from https://metakermit.com/2017/compiling-tensorflow-with-gpu-support-on-a-macbook-pro/

export CUDA_HOME=/usr/local/cuda
export DYLD_LIBRARY_PATH=/usr/local/cuda/lib:/usr/local/cuda/extras/CUPTI/lib
export LD_LIBRARY_PATH=$DYLD_LIBRARY_PATH
export PATH=$DYLD_LIBRARY_PATH:$PATH
# ==================================
```




## Steps

1. [**Install CUDA** from Nvidia](https://developer.nvidia.com/cuda-downloads) and [follow its official instructions](http://docs.nvidia.com/cuda/cuda-installation-guide-mac-os-x/index.html). Or, as suggested by [this post](https://metakermit.com/2017/compiling-tensorflow-with-gpu-support-on-a-macbook-pro/), install with `brew tap caskroom/drivers & brew cask install cuda`. 
   I installed from Nvidia, chose CUDA `8.0.61` and patched it to `8.0.62`. 

2. **Connect Graphic Card to Akitio Node and to laptop**, screw-driver needed. [Youtube Video](https://www.youtube.com/watch?v=MeOqTzGcgPI)

3. **Disable SIP** (System Integrity Protection). [Tutorial](http://osxdaily.com/2015/10/05/disable-rootless-system-integrity-protection-mac-os-x/ )

4. **Change OS Build Version** (required for Macbook with build 16F2073). [Discussion on this](https://egpu.io/forums/implementation-guides/2017-15-macbook-pro-touchbar-gtx1080ti40gbs-tb3akitio-node-macos/). In short,  open the file  `/System/Library/CoreServices/SystemVersion.plist` and change the build number from `16F2073` to `16F73`. 

5. **Download the automate-eGPU.sh** script and execute it.

   ```bash
   sudo ./automate-eGPU.sh -url https://images.nvidia.com/mac/pkg/378/WebDriver-378.05.05.15f01.pkg
   ```

6. After restart, I **upgrade CUDA to 8.0.90**

7. **Downgrade Xcode to 8.2** and corresponding command line tools. You could download them from [Apple Developer website](https://developer.apple.com/download/more/). Newer versions of clang would give you [error](https://github.com/arrayfire/arrayfire/issues/1384) when compiling CUDA samples. 

   Verify by `clang —version` and `pkgutil --pkg-info=com.apple.pkg.CLTools_Executables`. Expecting `Apple LLVM version 8.0.0 (clang-800.0.42.1)` and `version: 8.2.0.0.1.1480973914`.

8. **Add CUDA binaries to path**

   ```bash
   export PATH=/Developer/NVIDIA/CUDA-8.0/bin${PATH:+:${PATH}}
   export DYLD_LIBRARY_PATH=/Developer/NVIDIA/CUDA-8.0/lib${DYLD_LIBRARY_PATH:+:${DYLD_LIBRARY_PATH}}
   ```

9. **Verify CUDA installation** by running deviceQuery

   ```bash
   cd /usr/local/cuda/samples
   sudo make -C 1_Utilities/deviceQuery
   ./1_Utilities/deviceQuery
   ```

   should detect the device and yield `CUDA Capability Major/Minor version number: 6.1` ( I have a Geforce GTX 1080 Ti). 


10. **Install cuDNN v6.0 for Mac OS**

     - Download and unzip cuDNN 6.0 for MacOS from NVIDIA

     - Move the cuDNN libraries to cuda:

     ```bash
     sudo mv -v cuda/lib/libcudnn* /usr/local/cuda/lib
     sudo mv -v cuda/include/cudnn.h /usr/local/cuda/include
     sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib/libcud
     ```

     - Add to path

     ```bash
     export DYLD_LIBRARY_PATH="/usr/local/cuda/lib":$DYLD_LIBRARY_PATH
     ```

     - Verify the installation by `echo -e '#include"cudnn.h"\n void main(){}' | nvcc -x c - -o /dev/null -I/usr/local/cuda/include -L/usr/local/cuda/lib -lcudnn`.

     - I got a few warning but no error.



11. **Installing tensorflow** from Source, following the [official doc](https://www.tensorflow.org/install/install_sources#prepare_environment_for_mac_os) except the following:

      - **Bazel version**: The most recent r1.3 branch of tensorflow asks for bazel version 0.5.4. I got `xxx file not built` error when building with bazel 0.5.4. Therefore, I  `cd tensorflow & git checkout b46340f` and use **bazel 0.4.5** that comes with conda to build.

      - Set the environment flag

       ```bash
       export CUDA_HOME=/usr/local/cuda
       export DYLD_LIBRARY_PATH=/usr/local/cuda/lib:/usr/local/cuda/extras/CUPTI/lib
       export LD_LIBRARY_PATH=$DYLD_LIBRARY_PATH
       export PATH=$DYLD_LIBRARY_PATH:$PATH
       ```

      - **After checkout, comment out the BUILD file requiring OpenMP**: open `tensorflow/third_party/gpus/cuda/BUILD.tpl` and comment out `# linkopts = [“-lgomp”]` 

      - **Checksum mismatch** [discussion on Github](https://github.com/tensorflow/tensorflow/issues/12979). Workaround: _If it happens_, comment out the checksum line in `tensorflow/workspace.bzl` that says `sha256=repo_ctx_attr.sha256`.

      - **run the configuration**, opt in for CUDA support, and substitute `TF_CUDA_COMPUTE_CAPABILITIES` with your output of deviceQuery.

      - `bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package --verbose_failures --action_env PATH --action_env LD_LIBRARY_PATH --action_env DYLD_LIBRARY_PATH` 

         Took about 50 minutes and huge RAM consumption. Expecting no error. 

      - **Build the pip package** `bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg`

      - **Install the pip package** `sudo pip install /tmp/tensorflow_pkg/tensorflow-1.3.0-.whl`


12. **ALL DONE!**

      - Test your installment by running the [cutest MNIST toy](https://github.com/tensorflow/tensorflow/blob/r1.3/tensorflow/examples/tutorials/mnist/mnist_deep.py)

      - See, in your terminal, that tensorflow is running!
      ```bash
      2017-09-26 21:34:32.053838: I tensorflow/core/common_runtime/gpu/gpu_device.cc:976] DMA: 0
      2017-09-26 21:34:32.053843: I tensorflow/core/common_runtime/gpu/gpu_device.cc:986] 0:   Y
      2017-09-26 21:34:32.053851: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1045] Creating TensorFlow device (/gpu:0) -> (device: 0, name: GeForce GTX 1080 Ti, pci bus id: 0000:85:00.0)
      ```

    ​

    