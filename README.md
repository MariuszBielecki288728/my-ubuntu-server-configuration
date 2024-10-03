# my-ubuntu-server-configuration

An autoinstall files used for installing Ubuntu Server

# How to use?
Do you find autoinstall documentation confusing? Do you want to just try autoinstall feature on your home server and do not do some back-flips just to attach autoinstall.yaml to the ISO or the installation USB stick?

Try hosting it with python http server. The PC you want to install will need to be conntected to the same LAN network as the PC you are running server on.

1. Firstly create an .env file with variables referenced in `autoinstall_template.yaml`. Then

    ```shell
    mkdir server
    eval $(cat .env) envsubst < "autoinstall_template.yaml" > "server/user-data"
    touch server/meta-data
    cd server
    python3 -m http.server 3003
    ```

    Don't stop the server for now.

1. Create an installation media with standard Ubuntu Server ISO. See https://ubuntu.com/download/server#how-to-install
1. Attach the new installation media to your PC.
1. Boot up the PC with display, change boot order to boot from USB.
1. First that will boot the [GRUB](https://www.gnu.org/software/grub/) bootloader. 

    **This is the place you can configure autoinstall:** Find option to test and install Ubuntu and edit the boot command for this oprion (There should be an instruction at the bottom)
1. Find line with: `linux /casper/vmlinuz ---` and change it to add autoinstall configuration from the URL, which consists of LAN IP address of the computer you have run the python server on -> `linux /casper/vmlinuz autoinstall ds=nocloud-net;s=http://192.168.X.X:3003/ ---`.
1. Follow instructions on the bottom to boot the changed option (Probably bu CTRL+X shortcut)
