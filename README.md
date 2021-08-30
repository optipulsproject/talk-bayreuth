An optimal control problem for single spot pulsed laser welding
===============================================================

Joint Workshop of GAMM Activity Groups "Dynamics and Control Theory" and "Optimization with Partial Differential Equations" in Bayreuth, 2021.

**Authors:** Dmytro Strelnikov <dmytro.strelnikov@math.tu-chemnitz.de>, Roland Herzog <roland.herzog@math.tu-chemnitz.de>  
**License:** [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/legalcode)  

This repository contains fully reproducible slides for a talk which is a part of [OptiPuls][projectpage] project.


### Prerequisites (for local build only)

A working [FEniCS](https://fenicsproject.org/) computing platform installation is required as well as the following additional python packages (including their dependencies):

- [optipuls](https://github.com/optipulsproject/optipuls)
- [matplotlib](https://pypi.org/project/matplotlib/)
- [tabulate](https://pypi.org/project/tabulate/)

We suppose that [make](https://www.gnu.org/software/make/) is already installed on your machine provided a UNIX-like system is used.

If you already have FEniCS installed locally, you can use python virtual environments to install the remaining dependencies without cluttering your system:
```
python3 -m venv --system-site-packages ~/.local/optipuls
source ~/.local/optipuls/bin/activate
pip install git+https://github.com/optipulsproject/optipuls
pip install matplotlib tabulate
```

Since it can get quite tricky to install FEniCS, we also provide a bundle of docker images.


### Reproducing (local build)

Prebuilt [optipuilsproject](https://hub.docker.com/orgs/optipulsproject) images can be used to reproduce the results provided docker is installed on your system.

Once the depencdencies are satisfied, reproducing of the results is as simple as running `make` in the root of the project:
```
git clone https://github.com/optipulsproject/bayreuth-talk.git
cd bayreuth-talk
make -j$(nproc) slides
```

Make will run the computations, produce the plots, the tables, and the final `slides-bayreuth.pdf` file.


### Reproducing (local build in docker)

Make plots (entails making of the numerical artifacts):
```
docker run \
  -v $(pwd):/home/fenics/shared \
  optipulsproject/optipuls:latest \
  make plots.all -j$(nproc)
```

Make tables:
```
docker run \
  -u $UID \
  -v $(pwd):/data \
  optipulsproject/tabulate:latest \
  make tables.all
```

Make slides:
```
docker run \
  -u $UID \
  -v $(pwd):/data \
  optipulsproject/publications:latest \
  make slides
```


### Reproducing (local build in docker using precomputed artifacts)

In order to not carry the heavy computations locally, you may [download][gitlab-numericals-download] the latest numerical artifacts built by GitLab CI.

1. Clone the repocitory and open its directory:
```
git clone https://github.com/optipulsproject/bayreuth-talk.git
cd bayreuth-talk
```

2. Unpack the downloaded numerical artifacts into `bayreuth-talk` directory:
```
unzip -o artifacts.zip
```

3. Update the modification time of the numerical artifacts so they are treated up-to-date:
```
touch numericals/{rampdown,rampdown-noopt,zeroguess}/*
```

4. Run the steps of the previous section. The numerical artifacts won't be recomputed.


### GitLab CI/CD artifacts

- `manuscript-numapde-preprint.pdf` (latest successful) [view][gitlab-pdf-view], [download][gitlab-pdf-download]
- numericals (latest successful), [download][gitlab-numericals-download]



[projectpage]: https://www.tu-chemnitz.de/mathematik/part_dgl/projects/optipuls/index.en.php "OptiPuls"

[gitlab-pdf-view]: https://gitlab.hrz.tu-chemnitz.de/numapde/projects/202005-aif-dvs-optipuls/talk-bayreuth/-/jobs/artifacts/master/file/manuscript-numapde-preprint.pdf?job=tex
[gitlab-pdf-download]: https://gitlab.hrz.tu-chemnitz.de/numapde/projects/202005-aif-dvs-optipuls/talk-bayreuth/-/jobs/artifacts/master/raw/manuscript-numapde-preprint.pdf?job=tex
[gitlab-numericals-download]: https://gitlab.hrz.tu-chemnitz.de/numapde/projects/202005-aif-dvs-optipuls/talk-bayreuth/-/jobs/artifacts/master/download?job=numericals
