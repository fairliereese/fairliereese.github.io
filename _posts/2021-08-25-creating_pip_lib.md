---
title: 'Creating a PyPi library'
date: 2022-07-07 00:00:00
featured_image: '/images/img/pip.png'
sidebar_image: '/images/img/pip.png'
excerpt: Creating a pip installable python library
---

While writing my first python software library, [Swan](https://github.com/mortazavilab/swan_vis/tree/master/swan_vis), I struggled to figure out how to upload my library to the Python Package Index (PyPi, or `pip`), to make it installable from the command line. Hopefully these instructions will help someone else that's interested in learning how.

## Uploading your library to PyPi for the first time

1. Create your library locally with the directory structure `my_library/my_library`. Your `.py` files should be in `my_library/my_library`.
2. Make sure your library is importable (ie `import my_library`).
3. Create a PyPi account [here](https://pypi.org/account/register/) if you don't already have one.
4. In the directory above your `.py` files, (ie `my_library/`), create the following files:
  * `setup.py`
  * `setup.cfg`
  * `LICENSE.txt`
  * (optional) `README.txt`
5. Edit `setup.py`.
  * See [here](https://drive.google.com/file/d/17uLy3h0F4cl2AlLXVWC8sTClei19DeLI/view?usp=sharing) for an example and instructions on what to put in this file
  * See [here](https://pypi.org/classifiers/) for a list of valid classifiers you can use in this file
  * **Note**: You can automatically generate a list of python libraries that are required for yours to run (necessary for the  `install_requires` section of `setup.py`) using [`pipreqs`](https://github.com/bndr/pipreqs). This will automatically write all imported packages in your library to a `requirements.txt` file. These requirements are strict and you may want to relax these requirements if you believe your library will work with other versions of these requirements.
```bash
pip install pipreqs
pipreqs ~/mortazavi_lab/bin/swan_vis/swan_vis/
```

6. Edit `setup.cfg`. You can likely just use [this file](https://drive.google.com/file/d/1SdZNtID50sK_ptijfB8xog-2wu1GQIg7/view?usp=sharing).
7. Edit `LICENSE.txt`. If you want to use the MIT license, use [this file](https://drive.google.com/file/d/1H-pXVF5Di1Kl0nV8-jgkGKyOsIJrHqSG/view?usp=sharing).
8. Install `setuptools`
```bash
pip install setuptools
```
9. In the directory `my_library/`, run:
```bash
python setup.py sdist
```
10. Install `twine`, which will upload your library to PyPi
```bash
pip install twine
```
11. Make another account for [TestPyPi](https://test.pypi.org/account/register/ ) if you don't have one already (TestPyPi and PyPi are separate from one another).
12. Upload your library to TestPyPi first by running the following command in the `my_library/` directory:
```bash
twine upload --repository-url https://test.pypi.org/legacy/ dist/*
```
13. From any directory on your computer, test your upload by installing from TestPyPi:
```bash
pip install -i https://test.pypi.org/pypi/ --extra-index-url https://pypi.org/simple my_library
```
14. Try to import your newly-installed library in python. Start python and run:
```python
import my_library
```
You can and should also try some simple tests using the functionality of your library to ensure that it works.
15. After verifying that it worked, you are ready to upload your library to real PyPi. Run the following line of code in the `my_library/` directory:
```bash
twine upload dist/*
```
16. Now, you should be able to pip install your library from anywhere, and any computer (with internet access) using:
```bash
pip install my_library
```


## Updating your library on PyPi
1. Create a new release of your library on GitHub
2. Replace the `download_url` in `setup.py` with the link to the `tar.gz` file containing your release on GitHub.
3. Replace `version` in `setup.py` with the new version number that you assigned to your release on GitHub.
4. Remove the old package versions from your previous PyPi upload including the following folders:
```
dist/
my_library.egg_info/
```
If you don't remove these, `twine` will try to reupload an old distribution, and PyPi cannot accommodate two libraries with the same name / version ID.
5. Run the following code in `my_library/`
```bash
python setup.py sdist
```
6. Upload your library to TestPyPi and test it by running steps 12-14 from above.
7. Upload your library to normal PyPi by running step 15 above



References / additional resources:
* [Uploading your python package to PyPi](https://medium.com/@joel.barmettler/how-to-upload-your-python-package-to-pypi-65edc5fe9c56)
* [Test PyPi](https://packaging.python.org/en/latest/guides/using-testpypi/)
* [Building conda packages from PyPi](https://docs.conda.io/projects/conda-build/en/latest/user-guide/tutorials/build-pkgs-skeleton.html)
* [PyPi cannot accommodate libraries with the same name / version number](https://pypi.org/help/#file-name-reuse)
* [Writing the `setup.py` script](https://docs.python.org/3.7/distutils/setupscript.html)
* [Automatically creating `requirements.txt`](https://stackoverflow.com/questions/31684375/automatically-create-requirements-txt)
* [Installing my library from TestPyPi but the rest of the requirements from normal PyPi](https://stackoverflow.com/questions/34514703/pip-install-from-pypi-works-but-from-testpypi-fails-cannot-find-requirements)
* [Creating GitHub releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)
* The mistakes I made along the way
