![title](https://github.com/yanivsetton/android-docker/blob/master/Docker_Android.png

# Android on Docker
Building the SDK image on any **UNIX.**

**Requirements:**
> * Docker installed</br>
> * Java installed

## Step 1 - Clone the repo
```
git clone https://github.com/yanivsetton/android-docker.git
cd android-docker

```

## Step 2 - Update the Android version
```
cd sdk/tools/bin/
./sdkmanager "platform-tools" "platforms;android-<api_level>" "emulator"

Example:
sdkmanager "system-images;android-25;google_apis;armeabi-v7a" " "system-images;android-24;google_apis;armeabi-v7a"

# Please check sdkmanager --list to get the full options.
```

## Step 3 - Creating the emulator
```
# To create emulator in the path "android-docker/sdk/tools/bin"
echo "no" | avdmanager create avd -n test -k "system-images;android-25;google_apis;armeabi-v7a"
# Change the name "test" to feet your needs.
```

## Step 4 - Building the docker image
```
# navigate to the root path of the repository (android-docker)
docker build -t="yanivsetton/android-ready-sdk-vnc:latest" . # Change the name to feet your needs.
```

## Step 5 - Running the image
```
docker run -d --name android-emulator -p 5901:5901 -p 2222:22 yanivsetton/android-ready-sdk-vnc:latest:latest

# You can ADB on port 5037, SSH on port 2222, remote VNC on port 5901 or EXEC: docker exec -it android-emulator bash
# Inside the container you will already have the downloaded sdk veresion and licence accepted 
# you will have to create avd emulator with the downloaded image of the sdk
# and run the emulator.

# To run the emulator:
emulator64-arm -avd test -noaudio -no-boot-anim -accel on -gpu swiftshader_indirect &

# Please note that there is mant kinds of emulator to feet different architectures so use on depend your arch is.
# Please pay attention to the -avd "test" and replace with the relevant name.
```

### Using VNC
```
To use VNC open any VNC client and connect to the IP:5901
The password for full remote privileges is: android
The password for view only is: docker

You can change the passwords in the vncpass.sh file
```

### Using volume
```
Instead building image for any SDK version you can build the image without updating any SDK version.
In your HOST cd to android-docker/tools/bin
and update the required SDK versions using sdkmanager "platform-tools" "platforms;android-<api_level>" "emulator" (see step 2)
to use the volume add the following:

-v android-docker/sdk/:/opt/android-sdk/
so the finall command should be like:
docker run -d --name android-emulator -p 5901:5901 -p 2222:22 -v android-docker/sdk/:/opt/android-sdk/ yanivsetton/android-ready-sdk-vnc:latest:latest
```
