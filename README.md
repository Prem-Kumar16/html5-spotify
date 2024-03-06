# html5-spotify
Spotify version to run in AGL html5 version

## Steps for creating a webapp
- Create the new git repository (or local directory) for the new application
- Add a main html file, all needed resources, etc
- Choose and add a license file. It will be needed later when we create the recipe
- Add an appconfig.json file
- Create a Yocto recipe and add it to AGL tree

You can get the appinfo.json and index.html file from this repository.

## Creating the yocto recipe
- Create a recipe folder in /meta-agl-demo/recipes-demo/. For example, I have created a folder called html5-spotify.
- Create the recipe file: html5-spotify_git.bb
- Add the basic information (You can check the glossary for the meaning of each variable, and check Yocto guides). I have a sample bitbake recipe file in one of my repository called [html5-spotify-recipe](https://github.com/Prem-Kumar16/html5-spotify-recipe/tree/main).

### Important - Obtain LIC_FILES_CHKSUM value 
The `LIC_FILES_CHKSUM` value in a BitBake recipe is the checksum of the license file. It's used to ensure the license file hasn't changed. Here's how you can get it:

1. Download the license file from the project repository. In your case, the license file would be located at `https://github.com/rogerzanoni/html5-jamendo/blob/main/LICENSE`.

2. Calculate the MD5 checksum of the license file. You can do this using the `md5sum` command in a Unix-like operating system:

```bash
md5sum LICENSE
```

3. The output of the command will be the MD5 checksum of the file. This is the value you should use for `LIC_FILES_CHKSUM`. The format is `file://<filename>;md5=<checksum>`. For example:

```bash
LIC_FILES_CHKSUM = "file://LICENSE;md5=xxxxx"
```

Replace `xxxxx` with the actual MD5 checksum¹. Please note that the path to the license file is relative to the BitBake recipe file. If the license file is not in the same directory as the recipe file, you'll need to adjust the path accordingly. 

Remember, `LIC_FILES_CHKSUM` is mandatory in every recipe, unless `LICENSE` is set to `CLOSED`².

### Important - Obtain SRCREV value 

1. Navigate to your local clone of the repository. If you haven't cloned it yet, you can do so with the following command:

```bash
git clone <repo_url>
```

2. Once you're in the repository, you can get the latest commit hash (which is what `SRCREV` usually represents) with this command:

```bash
git rev-parse HEAD
```

This command will output the commit hash of the latest commit on the current branch.

If you want the commit hash of a specific branch, you can use:

```bash
git rev-parse <branch-name>
```

Replace `<branch-name>` with the name of the branch you're interested in¹.

Remember, `SRCREV` is usually set to a specific commit hash when you want to build a specific version of the software. If you always want to build the latest version, you can set `SRCREV` to `${AUTOREV}` in your BitBake recipe¹. This tells BitBake to always use the latest commit from the branch specified in `SRC_URI`¹.

## Changing the html5 packagegroup
- The AGL build system needs to know where the new application is to bake it into new images. For that, an entry needs to be added to "<build_dir>/meta-agl-demo/recipes-platform/packagegroups/packagegroup-agl-demo-platform-html5.bb"

```bash
nano packagegroup-agl-demo-platform-html5.bb
```

Inside that .bb file, include the app folder name (the name of the recipe folder in /meta-agl-demo/recipes-demo/. Here it is html5-spotify) like the image below

![Screenshot (518)](https://github.com/Prem-Kumar16/html5-spotify/assets/75419846/817e14c9-1f15-4a07-800d-6bb1381f0e07)


## Testing the new app on a Raspberry pi 4 image
- After the packagegroup setup, the application should be ready to be build and included in the image.
- Use "raspberrypi4" as architecture and build as you normally would. For reference, go through the [AGL docs](https://docs.automotivelinux.org/en/pike/)
