# Set up local environment

The Quix Portal offers you an [online
IDE](../platform/definitions.md#_online_ide), ready to use **without
any additional setup** to develop or deploy your applications. However,
you might sometimes want to work with your application in a local IDE.

In such cases, Python development needs some setup before you can use
our SDK. In this section are detailed instructions of how to set up your
Python environment on Linux, Mac or Windows.

!!! note

	This guide assumes you have already cloned a project of your own or download a library sample provided within the platform. In addition some stages may require files from [GitHub](https://github.com/quixai/quix-library/tree/main/python/local-development).

!!! warning

	Make sure you’re using Python version 3.7\>=, \<3.9

## Install Dependencies

To get started, install the SDK dependencies.

=== "MacOS Intel"
    You can install dependencies via a script or manually.
    
    **Install dependencies via script**
    
    Download the script named `quix-dependency-installer-mac.sh` from
    [GitHub](https://github.com/quixai/quix-library/tree/main/python/local-development),
    which installs all necessary requirements. Download the project then
    run the script by copy-pasting the following into a terminal from
    the project’s top directory.
    
    ``` bash
    chmod +x ./quix-dependency-installer-mac.sh && ./quix-dependency-installer-mac.sh
    ```
    
    **Install dependencies manually**
    
      - Install and configure PythonNet dependencies
        
          - Install the Brew package manager (from
            [brew.sh](https://brew.sh/)). To add brew to your path:
            
            ``` bash
            echo "export PATH=/usr/local/bin:$PATH" >> ~/.bash_profile && source ~/.bash_profile && echo "Worked" || echo "Failed"
            ```
        
          - Install Mono Version
            
            ``` bash
            brew install mono
            echo "export PATH=/Library/Frameworks/Mono.framework/Versions/Current/Commands:$PATH" >> ~/.bash_profile && source ~/.bash_profile && echo "Worked" || echo "Failed"
            echo "export PKG_CONFIG_PATH=/Library/Frameworks/Mono.framework/Versions/Current/lib/pkgconfig:$PKG_CONFIG_PATH" >> ~/.bash_profile && source ~/.bash_profile && echo "Worked" || echo "Failed"
            ```
    
      - Additional things to install:
        
        ``` bash
        brew install pkg-config
        python3 -m pip install wheel
        python3 -m pip install pycparser
        ```

=== "MacOS M1"
    You can install dependencies via a script or manually.
    
    **Install dependencies via script**
    
    Download the script named `quix-dependency-installer-mac.sh` from
    [GitHub](https://github.com/quixai/quix-library/tree/main/python/local-development),
    which installs all necessary requirements. Download the project then
    run the script by copy-pasting the following into a terminal from
    the project’s top directory.
    
    ``` bash
    chmod +x ./quix-dependency-installer-mac.sh && ./quix-dependency-installer-mac.sh
    ```
    
    **Install dependencies manually**
    
      - Install and configure PythonNet dependencies
        
          - Install the Brew package manager (from
            [brew.sh](https://brew.sh/)). To add brew to your path:
            
            ``` bash
            echo "export PATH=/opt/homebrew/bin:$PATH" >> ~/.bash_profile && source ~/.bash_profile && echo "Worked" || echo "Failed"
            ```
        
          - Install rosetta
            
            ``` bash
            sudo softwareupdate --install-rosetta --agree-to-license
            ```
        
          - Install Mono Version
            
            ``` bash
            curl https://download.mono-project.com/archive/6.12.0/macos-10-universal/MonoFramework-MDK-6.12.0.122.macos10.xamarin.universal.pkg -O
            monoPkgPath=./MonoFramework-MDK-6.12.0.122.macos10.xamarin.universal.pkg
            sudo installer -pkg $monoPkgPath -target /
            echo "export PATH=/Library/Frameworks/Mono.framework/Versions/Current/Commands:$PATH" >> ~/.bash_profile && source ~/.bash_profile && echo "Worked" || echo "Failed"
            echo "export PKG_CONFIG_PATH=/Library/Frameworks/Mono.framework/Versions/Current/lib/pkgconfig:$PKG_CONFIG_PATH" >> ~/.bash_profile && source ~/.bash_profile && echo "Worked" || echo "Failed"
            ```
    
      - Additional things to install:
        
        ``` bash
        brew install pkg-config
        python3 -m pip install wheel
        python3 -m pip install pycparser
        ```

=== "Windows"  
    Install the latest .Net Core runtime
    (<https://dotnet.microsoft.com/download/dotnet-core/current/runtime>)

=== "Linux"
    
    !!! note
    
		Because of the variability between linux distributions we highly recommend using a Docker environment ( see [Docker setup](docker-setup.md) ) in case you experience any issues during the installation.
    
      - Install and configure PythonNet dependencies - Ubuntu 20.04
        
          - Install Mono version 6.10.0
    
    <!-- end list -->
    
    ``` bash
    sudo apt install gnupg ca-certificates
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
    echo "deb https://download.mono-project.com/repo/ubuntu stable-focal/snapshots/6.10.0 main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list && apt-get update
    sudo apt update
    sudo apt install mono-devel
    ```
    
    For other linux distributions, please see the Mono [current release
    page](https://www.mono-project.com/download/stable/#download-lin)
    and [older Releases
    page](https://www.mono-project.com/docs/getting-started/install/linux/#accessing-older-releases)
    
      - Additional things to install:
    
    <!-- end list -->
    
    ``` bash
    sudo apt install python3-pip clang libglib2.0-0 build-essential librdkafka-dev
    python3 -m pip install pycparser
    ```
    
    In some distributions librdkafka-dev will not be readily available
    and guide (<https://github.com/edenhill/librdkafka>) must be
    followed. For ubuntu 20.04, the following steps are readily curated
    for you:
    
    ``` bash
    sudo apt-get install -y wget software-properties-common
    wget -qO - https://packages.confluent.io/deb/7.2/archive.key | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://packages.confluent.io/deb/7.2 stable main"
    sudo add-apt-repository "deb https://packages.confluent.io/clients/deb $(lsb_release -cs) main"
    sudo apt-get update
    sudo apt install librdkafka-dev -y
    ```



## Create New Python Environment

It’s good practice to use a python virtual environment and it is doubly
true when using the Quix streaming library. The library currently relies
on some dll redirecting, which is achieved by adding a file to your
python environment. This is done automatically, but if you have other
python application(s)/package(s) that also rely on similar redirection
then a virtual environment is advised.



=== "MacOS"
    To create a new virtual environment, execute the following in a
    terminal at your desired location (such as the root folder of the
    downloaded sample):
    
    ``` bash
    python3 -m pip install virtualenv
    python3 -m virtualenv env --python=python3.8.7
    chmod +x ./env/bin/activate
    source ./env/bin/activate
    ```
    
    You will know you succeeded in activating the environment if your
    terminal line starts with (env). Future steps will assume you have
    the virtual environment activated or are happy to install globally.

=== "Windows"
    To create a new virtual environment, execute the following in a
    terminal at your desired location (such as the root folder of the
    downloaded sample):
    
    ``` bash
    pip install virtualenv
    python -m virtualenv env --python=python3.8
    "env/Scripts/activate"
    ```
    
    You will know you succeeded in activating the environment if your
    command line starts with (env). Future steps will assume you have
    the virtual environment activated or are happy to install globally.
    
    !!! note
    
		You might need to use a new command line after installing Python, because PATH isn’t refreshed for existing command lines when something is installed.

=== "Linux"
    To create a new virtual environment, execute the following in a
    terminal at your desired location:
    
    ``` bash
    python3 -m pip install virtualenv
    python3 -m virtualenv env --python=python3.8
    chmod +x ./env/bin/activate
    source ./env/bin/activate
    ```
    
    You will know you succeeded in activating the environment if your
    terminal line starts with (env). Future steps will assume you have
    the virtual environment activated or are happy to install globally.



## Install code requirements



=== "MacOS"
    In the same terminal you activated the virtual environment, navigate
    to the folder where `requirements.txt` (in the sample you
    downloaded) is located and execute the following:
    
    ``` bash
    python3 -m pip install -r requirements.txt --extra-index-url https://pkgs.dev.azure.com/quix-analytics/53f7fe95-59fe-4307-b479-2473b96de6d1/_packaging/public/pypi/simple/
    ```

=== "Windows"
    In the same console you activated the virtual enviornment, navigate
    to the folder where `requirements.txt` (in the sample you
    downloaded) is located and execute the following:
    
    ``` bash
    pip install -r requirements.txt --extra-index-url https://pkgs.dev.azure.com/quix-analytics/53f7fe95-59fe-4307-b479-2473b96de6d1/_packaging/public/pypi/simple/
    ```

=== "Linux"
    In the same terminal you activated the virtual environment, navigate
    to the folder where `requirements.txt` (in the sample you
    downloaded) is located and execute the following:
    
    ``` bash
    python3 -m pip install -r requirements.txt --extra-index-url https://pkgs.dev.azure.com/quix-analytics/53f7fe95-59fe-4307-b479-2473b96de6d1/_packaging/public/pypi/simple/
    ```



Your environment should be ready to run the code\!



=== "MacOS"
    
    ``` bash
    python3 main.py
    ```

=== "Windows"
    
    ``` bash
    python main.py
    ```

=== "Linux"
    
    ``` bash
    python3 main.py
    ```


