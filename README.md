nVidia CUDA Runtime 11.2 for Core18

This snap only provides implementation for amd64/x86_64. All other
architectures are empty.

To consume this runtime snap you must add the following plug configuration:

```yaml
plugs:
  cuda-runtime-11-2-1804:
    interface: content
    target: $SNAP/cuda-11.2
    default-provider: cuda-runtime-11-2-1804

environment:
  LD_LIBRARY_PATH: $SNAP/cuda-11.2/usr/lib/$SNAPCRAFT_ARCH_TRIPLET:$SNAP/cuda-11.2/local/cuda-11.2/lib64
```

You will also need `cuda-11-2` added to your `build-packages` if you
want to compile against the SDK, and make sure you don't stage any of
the cuda libraries directly:

```yaml
package-repositories:
  - type: apt
    formats: [deb]
    path: /
    key-id: AE09FE4BBD223A84B2CCFCE3F60F4B3D7FA2AF80
    url: https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64

parts:
  your-part:
    ...
    build-packages:
      - cuda-11-2 # install the CUDA SDK to build against
    stage:
      - -usr/local/cuda-11.2/* # make sure we don't include CUDA libs
```