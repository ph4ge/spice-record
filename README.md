[spice-record]
============
This is a simple utility for recording a [SPICE] sesion to MP4 video.
It uses libvirt to connect to the VMs, `SpiceClientGLib` to access the graphics
device, and FFmpeg to encode MP4 videos.

## Usage
```
usage: spice-record [-h] [--vcodec VCODEC]
                    [--loglevel {DEBUG,INFO,WARNING,ERROR,CRITICAL}]
                    [-r FRAMERATE] [-c LIBVIRT_URI] [-o FILENAME]
                    DOMAIN-NAME|ID|UUID

positional arguments:
  DOMAIN-NAME|ID|UUID   Machine to record

optional arguments:
  -h, --help            show this help message and exit
  --vcodec VCODEC       Set the output video codec (see "ffmpeg -encoders" for
                        choices)
  --loglevel {DEBUG,INFO,WARNING,ERROR,CRITICAL}
                        Set the logging level (default=WARNING)
  -r FRAMERATE, --framerate FRAMERATE
  -c LIBVIRT_URI, --connect LIBVIRT_URI
                        Connect to hypervisor (e.g. qemu:///system)
  -o FILENAME, --output FILENAME
                        Output filename (defaults to <domain-name>.mp4)
```

## Requirements
- Python 3
- `libvirt-python` (not `libvirt-glib`)
- `spice-glib`
- `pygobject3`
- `ffmpeg`

If `virt-manager` is installed on a modern distro (which has ported all of its
Python apps to Python 3), then everything should already be installed, aside
from `ffmpeg`.

---

Handling requirements on **Ubuntu** jammy:

`libvirt-python`

```bash
sudo apt update && sudo apt install libvirt-dev -y
```

`spice-glib`

```bash
sudo apt update && sudo apt install libspice-client-glib-2.0-dev -y
```

`pygobject3` or `PyGObject`

```bash
sudo apt update && sudo apt install ffmpeg libgirepository1.0-dev gcc libcairo2-dev pkg-config python3-dev gir1.2-gtk-4.0 -y
```

`ffmpeg`

```bash
sudo apt update && sudo apt install ffmpeg -y
```

Finally, resolving `pypi` dependencies:

```bash
pip3 install -r requirements.txt
```


## Notes
Currently, the spice server only supports a single client connection. When
another connection is opened, the current one is disconnected. Thus, this
utility is limited in its usability as it cannot record a user interacting with
the VM, and only an automatic ongoing process. There is however, an
experimental feature to enable [multiple concurrent
connections][MultipleClients] to a single spice server.

[spice-record]: https://github.com/JonathonReinhart/spice-record
[SPICE]: https://www.spice-space.org/
[MultipleClients]: https://www.spice-space.org/multiple-clients.html
