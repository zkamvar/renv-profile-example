# Example of renv profiles

I created this repository to serve as an example of using renv profiles. 
A profile is a folder in `renv/profiles/` that contains a `renv.lock` file and
a `renv/` library folder. The profile that renv chooses to use is dictated by
the `renv/profile` file, which contains the name of the profile renv should
use.

## Setup

I did the following to create it:

```bash
# 1. Create file for renv to seed off of
echo "aweek::as.aweek(Sys.Date())" > test.R
# 2. Initialize renv (this will download aweek)
R -e "renv::init()"
# 3. Add 'cowsay' profile by creating a folder
mkdir -p renv/profiles/cowsay
#    ... copying over the lockfile
cp renv.lock renv/profiles/cowsay
#    ... telling renv to use the cowsay profile on start
echo "cowsay" > renv/profile
#    ... recording the "cowsay" package (despite the fact that we do not use it)
R -e "renv::record('cowsay'); renv::restore(); renv::snapshot(packages = 'cowsay', update = TRUE)"
# 4. reset to use the default profile
echo "default" > renv/profile
```

## Usage

There are two ways to use a profile before you start R

1. set `renv/profile` to the profile you want to use by using `echo "PROFILE NAME" > renv/profile`. This sets it persistantly
2. use `RENV_PROFILE="PROFILE NAME"` before you run R or your editor. This is
   demonstrated in the GitHub workflow. 


> [!IMPORTANT]
>
> GitHub Runners will start in whatever profile is recorded in `renv/profile`,
> so make sure it's set to `default`



