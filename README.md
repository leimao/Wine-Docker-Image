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

To run the Docker container, please run the following command with additional Docker arguments if necessary.

```bash
$ xhost +
$ docker run \
    -it \
    --rm \
    --gpus all \
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

## Example

We will use running Windows WeChat as an example.

### Download WeChat

```bash
$ wget https://dldir1.qq.com/weixin/Windows/WeChatSetup.exe
```

### Run Docker Container

```bash
$ xhost +
$ docker run \
    -it \
    --gpus all \
    --device /dev/snd \
    --device=/dev/dri \
    -e DISPLAY=$DISPLAY \
    -e XMODIFIERS=@im=fcitx \
    -e QT_IM_MODULE=fcitx \
    -e GTK_IM_MODULE=fcitx \
    -v /tmp/.X11-unix:/tmp/.X11-unix:ro \
    -v $(pwd):/mnt \
    wine:20.04
```

Notice `--rm` has been removed and `-e XMODIFIERS=@im=fcitx`, `-e QT_IM_MODULE=fcitx`, `-e GTK_IM_MODULE=fcitx` has been added to enable the `fcitx` input from the host computer.

### Configure Wine

```bash
$ WINEARCH=win64 WINEPREFIX=~/.wine64 winecfg
```

Make sure virtual desktop is used in `winecfg`. In my case, I used resolution `2560 x 1440` for the virtual desktop.

### Install WeChat

```bash
$ cd /mnt
$ WINEPREFIX=~/.wine64 wine WeChatSetup.exe
```

### Run WeChat

```bash
$ WINEPREFIX=~/.wine64 wine ~/.wine64/drive_c/Program\ Files\ \(x86\)/Tencent/WeChat/WeChat.exe
```

