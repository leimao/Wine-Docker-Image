# Wine Docker Image

## Introduction

Basic Docker image for Wine.


## Usages

### Build Docker Image

To build the Docker image, please run the following command.

```bash
$ docker build -f wine.Dockerfile --tag=wine:20.04 .
```

### Run Docker Container

To run the Docker container, please run the following command.

```bash
$ xhost +
$ docker run \
    -it \
    --rm \
    --device /dev/snd \
    --device=/dev/dri \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix:ro \
    -v $(pwd):/mnt \
    wine:20.04
$ xhost -
```

### Configure Wine

For Win32 applications, we create a directory `~/.wine32` to store the configurations.

```bash
$ WINEARCH=win32 WINEPREFIX=~/.wine32 winecfg
```

For Win64 applications, we create a directory `~/.wine64` to store the configurations.

```bash
$ WINEARCH=win64 WINEPREFIX=~/.wine64 winecfg
```

### Run Application

For Win32 applications, we specify the Win32 configuration directory as `WINEPREFIX` before `wine`.

```bash
$ WINEPREFIX=~/.wine32 wine app.exe
```


For Win32 applications, we specify the Win64 configuration directory as `WINEPREFIX` before `wine`.

```bash
$ WINEPREFIX=~/.wine64 wine app.exe
```

### Run Display Scaling

For some old applications, the display usually is very small. We could scale the display using a scaling script.

For example, to scale the display of app.exe by 1.5x, we could run the following command.


```bash
$ WINEPREFIX=~/.wine32 run_scaled --scale=1.5 wine app.exe
```

