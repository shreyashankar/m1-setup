# Setting up a Mac with the M1 chip

*As of: April 21, 2021*

I recently got a new MacBook Pro with the M1 chip. Since Apple switched from Intel to ARM-based processors, I had to go through some hoops to get my development environment set up. Here are a few notes on the setup processes, but note that much of this may go out-of-date soon.

## Basics

| Application | Notes |
| ----------- | ----------- |
| Chrome      | Stable release for Apple Silicon [here](https://www.google.com/chrome/).       |
| 1Password   | Only the beta release seems to work, download [here](https://app-updates.agilebits.com/download/OPM7/Y).   |
| Slack | Stable release for Apple Silicon [here](https://slack.com/downloads/mac). |
| Zoom | Stable release for Apple Silicon [here](https://zoom.us/download). |
| Rosetta | You will need this to run many non-native apps. Run `softwareupdate --install-rosetta` in your terminal. |
| Spotify | Does not run natively. Install Rosetta first, then download normally from the Spotify website. |

## Productivity tools


| Application | Notes |
| ---- | ---- |
| NordVPN | Does not run natively. Install Rosetta first, then download normally from the App Store. |
| Spark (email client) | Does not run natively. Install Rosetta first, then download normally from the App Store. |
| RescueTime | Does not run natively. Install Rosetta first, then download normally from the App Store. |
| Bear notes | Seems to run natively; download normally from the App Store. |
| Fantastical | Seems to run natively; download normally from the App Store. |

## Developer essentials

| Utility | Notes |
| --- | --- |
| xcode command line utils | Run `xcode-select --install` in your terminal. Alternatively the terminal seems to prompt the user to download the xcode command line utils after you try to execute a command such as `git`. |
| zsh config | Looks like the default shell is zsh in MacOS Big Sur. I installed [ohmyzsh](https://github.com/ohmyzsh/ohmyzsh) normally using `curl` and it worked fine. |
| VSCode | Stable release for Apple Silicon [here](https://code.visualstudio.com/download). |
| Homebrew | Release for Apple Silicon [here](https://brew.sh). They say it's partial but it has worked for everything I needed. However, you must check out to master by additionally running the command `git -C $(brew —repo homebrew/core) checkout master` after you install brew. |
| `node` | Install brew as above, then run `brew install node` in your terminal. |
| `yarn` | Install brew as above, then run `brew install yarn` in your terminal. |
| `python` | **Do not** install with brew! Use miniforge, as this will make it infinitely easier to install `numpy` and other scientific computing libraries. Download the ARM64 version from [here](https://github.com/conda-forge/miniforge/#download), then run `bash Miniforge3-MacOSX-arm64.sh`. Then you can create environments as you would using conda (i.e. `conda create -n myenv python=3.8`) and `conda install` `numpy`/`scipy` whatever library of your choosing. |
| MacTeX | Believe it or not, there is a stable release for Apple Silicon [here](https://www.tug.org/mactex/mactex-download.html). |

## Engineering tools

| Tool | Notes |
| --- | --- |
| Docker Desktop | Install Rosetta first, then download the (stable) M1 version from [here](https://docs.docker.com/docker-for-mac/apple-silicon/). You need Rosetta because some binaries are still Darwin/AMD. Note that not all images are available for ARM64. |
| Postman | Does not run natively. Install Rosetta first, then download normally from the website. |
| Postgres and `psycopg2` | This one is really tricky! After lots of searching, I found working instructions in [this comment](https://github.com/psycopg/psycopg2/issues/1200#issuecomment-776159466). Install postgres via brew with `brew install postgresql`, then run `brew link openssl` and **make sure** you execute the exports in the output from this command. Then you should be able to `pip install psycopg2-binary` fine. If you're using `psycopg2` in docker, write your images based off `python:3.9` instead of `slim` images (see [this comment](https://github.com/psycopg/psycopg2/issues/1200#issuecomment-821823094)). |
| `gatsby` | Make sure you have `node` and `yarn` installed. Gatsby uses `libvips` which you will have to compile and install globally yourself. Follow steps in [this issue](https://www.gitmemory.com/issue/lovell/sharp/2460/739092532) to do this. Basically, you use Mac ports to install specific dependencies. |
| `tmux` | Install brew as above, then run `brew install tmux`. |
| `c`/`c++` | Covered by xcode command line tools. To verify, run `clang++ --version` in your terminal. |

## Data science tools

| Tool | Notes |
| --- | --- |
| `numpy`, `scipy`, `pandas`, `jupyterlab`, `jupyter` | Install conda (miniforge) as above. When you create a conda environment, you can run `conda install LIB_NAME` pretty smoothly. |
| `tensorflow` | To leverage the M1 chip (GPU and CPU), you will need to download Apple's `tensorflow_macos`. I successfully did this in a conda environment by following the instructions in [this issue](https://github.com/apple/tensorflow_macos/issues/153#issue-799924913). Basically, if you have conda (miniforge), you can write an `environment.yml` file based off [this file](https://raw.githubusercontent.com/mwidjaja1/DSOnMacARM/main/environment.yml) and create a conda environment with this file. `pip install` the wheels as described in the instructions and you should be good to go. <br> <br> To set the device to `gpu`, run `from tensorflow.python.compiler.mlcompute import mlcompute; mlcompute.set_mlc_device(device_name='gpu')`. It will give a warning that `gpu` does not work in eager mode, but I was able to train a neural network blazingly fast (compared to my Intel MacBook), and Activity Monitor showed GPU utilization. |
| `pytorch` | The `pytorch` team has not released a stable native version for Apple Silicon yet. The most hassle-free solution I found is to open the terminal with Rosetta and download cpu versions of `torch` and `torchvision`. However, if you really want to use your Apple Silicon and are happy with `torch` alone, the miniforge version of conda allows you to `conda install pytorch` or build from source using a Mac ARM64 version listed [here](https://download.pytorch.org/whl/torch_stable.html). Some people have successfully built `torch` and `torchvision` from source following the steps in [this issue](https://github.com/pytorch/pytorch/issues/48145#issuecomment-783094831), but I am too lazy to check if it works for me. |
| Microsoft Excel | Stable release for Apple Silicon can be downloaded through the Microsoft website or whatever university or organization allows you to download Microsoft Office. |

## Additional resources

* [Does it ARM?](https://doesitarm.com/)
* [Accelerating TensorFlow performance on Mac](https://blog.tensorflow.org/2020/11/accelerating-tensorflow-performance-on-mac.html)
* [PassMark benchmarks](https://www.cpubenchmark.net/singleThread.html)

Feel free to make a PR with edits to this README if you're interested in adding or updating instructions. 
