# arXiv
Get your daily arXiv digest

This will give you a complete list with title, author and abstract. You can then enter a list of numbers to download.
Filenames longer than the maximum filename length supported by the filesystem will be truncated with respect to the authors.

## Requirements

 - `python3`
 - `python-beautifulsoup4` (`pip install bs4` if you use `python-pip` package)
 - `wget`

## Setup
`chmod +x arxiv` and put it somewhere in your `PATH`.
See `echo $PATH` for locations or alter `PATH`.

arxiv listens on a config file in `.config/arxiv.conf`. If it is not present, it will be generated.

**E.g.**
```
[DEFAULT]
url = https://arxiv.org/list/cond-mat.mes-hall/recent, https://arxiv.org/list/q-bio/new, https://arxiv.org/list/astro-ph/recent
style = $authors-$title.pdf
```

If `style` is not set, `%title-%authors-%doi.pdf` will be set instead.

## Example

```
[...]

  120 Flavours in the box of chocolates: chemical abundances of kinematic  substructures in the nearby stellar halo
      Jovan Veljanoski, Amina Helmi

 Different subtleties and problems associated with a nonrelativistic limit of the field theory to the Schroedinger theory are discussed. In this paper, we revisit different cases of the nonrelativistic limit of a real and complex scalar field in the level of the Lagrangian and the equation of motion. We develop the nonrelativistic limit of the Dirac equation and action in the way that the nonrelativistic limit of spin-$\frac{1}{2}$ wave functions of particles and antiparticles appear simultaneously. We study the effect of a potential like $U(\phi)\propto \phi^4$ which can be attributed to axion dark matter field in this limit. We develop a formalism for studying the nonrelativistic limit of antiparticles in the quantum mechanics. We discussed the non-local approach for the nonrelativistic limit and its problems.

  121 The Masses and Accretion Rates of White Dwarfs in Classical and  Recurrent Novae
      Michael Shara, Dina Prialnik, Yael Hillman, Attay Kovetz

 Different subtleties and problems [...]

Download (2 12 ..): 1 3 5 6
```
