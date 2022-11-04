# renderman-dexed-linux

This is a tutorial for how to run [RenderMan](https://github.com/fedden/RenderMan) + [Dexed](https://github.com/asb2m10/dexed) on Linux (Ubuntu 18.04).
Using `xvfb` to work around the X Window System was the easiest.

## Install prerequisites

```
sudo apt-get update
sudo apt-get install libboost-all-dev llvm clang libfreetype6-dev libx11-dev libxinerama-dev libxrandr-dev libxcursor-dev mesa-common-dev libasound2-dev freeglut3-dev libxcomposite-dev libcurl4-gnutls-dev xvfb
```


## Build RenderMan


```
git clone https://github.com/fedden/RenderMan.git
cd RenderMan/Builds/LinuxMakefile/
perl -i -pe 's/-fvisibility=hidden/-fvisibility=default/' Makefile
make
```

## Build Dexed
```
cd
git clone --branch v0.9.4 https://github.com/asb2m10/dexed.git
cd dexed/Builds/Linux/
make
```

## Run

```
cd ~/RenderMan/Builds/LinuxMakefile/build/
xvfb-run python
```

Run the following Python code:

```python
engine.load_plugin('/your/home/dexed/Builds/Linux/build/Dexed.so', 0)
print(engine.get_plugin_parameters_description())
generator = rm.PatchGenerator(engine)
random_patch = generator.get_random_patch()
engine.set_patch(random_patch)
engine.render_patch(40, 127, 4.0, 5.0)
audio = engine.get_audio_frames()
```
