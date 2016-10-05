# FAQs

#### I am getting errors when running the `make` command !
Make sure you have followed the instructions given in our [Development Host Setup](http://marvell-iot.github.io/docs/#documentation-setup-development-host) guide. Please check -

- The `PATH` variable is set correctly, and you can access the toolchain from any location using the command prompt for your OS
- If you are on Windows, please make sure that you have `make` installed as per the instructions in the host setup guide. 

#### How do I erase everything on the board ?
One way to do that is to use the layout file that can be found in `sdk/tools/OpenOCD/mw300/` folder. Use the `flashprog.py` utility to erase all the contents from the flash

```
python sdk/tools/OpenOCD/flashprog.py -l sdk/tools/OpenOCD/mw300/layout.txt 
```

#### I want to change the configured Wi-Fi network. What should I do ?
Please check the steps mentioned in the above example. The Wi-Fi credentials are stored in the persistent memory/flash so that they can be accessed after a reboot. The `flashprog.py` utility presently does not have the provision to selectively erase the Wi-Fi credentials. 

#### How can I flash a `.blob` using `flashprog.py` ?
The older `flash.py` utility had the ability to flash a generated `.blob` file. The new `flashprog.py` allows you to flash only the partitions as mentioned in the layout file. You can read the flash from a dev board and write it back using the `--read` and the `--write` parameters, which should serve as a viable replacement for flashing `.blob` files using `flash.py`.

#### I have a question not listed above ! Help !
Head over to our Gitter.im channel [here](https://gitter.im/marvell-iot/aws_starter_sdk). You will need a GitHub account to sign in.

