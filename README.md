# Cloudjumper Payload

## Downloading

If you just want to run the code, download this repository by navigating to wherever you want to store it and running the following command:

```bash
git clone --recurse-submodules https://github.com/UMass-Lowell-Rocketry-Club/Cloudjumper-Payload.git
```

If you want to edit the repository, use this command instead:

```bash
git clone --recurse-submodules git@github.com:UMass-Lowell-Rocketry-Club/Cloudjumper-Payload.git
```

If you want to edit the repository you should also read the [setup instructions](https://github.com/UMass-Lowell-Rocketry-Club/Cloudjumper-Sensors/blob/main/SUBMODULE%20MAINTENANCE.md#editing-submodules).

## Updating

If you have already downloaded the repository to the payload, you can update it using the following command:

```bash
cd ~/Cloudjumper-Payload
git pull --recurse-submodules  # --recurse-submodules updates Cloudjumper-Sensors too
```

## Setting Up The Service

We use a `systemd` service to run the radio transmitter program after boot. After pulling changes to **the service configuration file**, there are a few commands that need to be run:

```bash
# copy the config file to the correct location
sudo cp config_files/radio-transmitter.service /etc/systemd/system
# tell systemd that the config file changed
sudo systemctl daemon-reload
# enable the radio-transmitter service at startup
sudo systemctl enable radio-transmitter
# start the radio-transmitter service right now (optional)
sudo systemctl start radio-transmitter
```

You can check the status of the service with `systemctl status radio-transmitter`. If you need to, you can stop it with `systemctl stop radio-transmitter`
