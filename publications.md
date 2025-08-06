---
title: Scientific Publications
parent: Secure Chain
nav_order: 6
---

# Scientific Publications

This section presents all the scientific publications that have resulted from the development of this organisation's tools.

## Vulnerability impact analysis in software project dependencies based on Satisfiability Modulo Theories (SMT)

**Abstract:** Software development projects are built on top of external libraries and tools that help manage code and databases and/or facilitate deployment. The external libraries that assist in these tasks create dependent relations with the developed software, thereby increasing the use of dependencies as a common practice. There exist mechanisms in the projects to set up software dependencies in terms of versions and restrictions between said projects. However, any problem, error, or vulnerability affecting a software's configuration dependencies can render the whole project vulnerable. This turns a secure dependency into an insecure dependency, and hinders the maintenance of security in software development projects, since current tools do not cover all possible configurations of dependencies. In this paper, our approach that enables the analysis and inference of the configuration of dependencies of projects in terms of potentially vulnerable configurations. The proposal is developed by constructing a dependency graph network attributed to vulnerabilities. Formal models are integrated based on Satisfiability Modulo Theories (SMT) to enable automatic analysis, such as the identification of the most secure configuration of dependencies. The automatic analysis facilitates ascertaining the vulnerability-free configurations of dependencies with maximum and minimum vulnerability impacts. This proposal has been evaluated by analysing more than 140 Python open-source code repositories and better results than other proposals have been achieved.

**Citation:**

```bibtex
@article{securechain-cose,
    title = {Vulnerability impact analysis in software project dependencies based on Satisfiability Modulo Theories (SMT)},
    journal = {Computers & Security},
    volume = {139},
    pages = {103669},
    year = {2024},
    issn = {0167-4048},
    doi = {https://doi.org/10.1016/j.cose.2023.103669},
    url = {https://www.sciencedirect.com/science/article/pii/S0167404823005795},
    author = {A. {Germán Márquez} and Ángel Jesús Varela-Vaca and María Teresa {Gómez López} and José A. Galindo and David Benavides},
    keywords = {Security, Vulnerability, Automated analysis, Satisfiability Modulo Theories (SMT), Dependency network, Software development}
}
```

## Depex: A software for analysing and reasoning about vulnerabilities in software projects dependencies

**Abstract**: This paper presents Depex, a tool that allows developers to reason over the entire configuration space of the dependencies of an open-source software repository. The dependency information is extracted from the repository requirements files and the package managers of the dependencies, generating a graph that includes information regarding security vulnerabilities affecting the dependencies. The dependency graph allows automatic reasoning through the creation of a Boolean satisfiability model based on Satisfiability Modulo Theories (SMT). Automatic reasoning lets operations such as identifying the safest dependency configuration or validating if a particular configuration is secure. To demonstrate the impact of the proposal, it has been evaluated on more than 300 real open-source repositories of Python Package Index (PyPI), Node Package Manager (NPM) and Maven Central (Maven), as well as compared with current commercial tools on the market.

**Citation**:

```bibtex
@article{securechain-softwarex,
    title = {Depex: A software for analysing and reasoning about vulnerabilities in software projects dependencies},
    journal = {SoftwareX},
    volume = {30},
    pages = {102152},
    year = {2025},
    issn = {2352-7110},
    doi = {https://doi.org/10.1016/j.softx.2025.102152},
    url = {https://www.sciencedirect.com/science/article/pii/S2352711025001190},
    author = {Antonio Germán Márquez and Ángel Jesús Varela-Vaca and María Teresa {Gómez López} and José A. Galindo and David Benavides},
    keywords = {Security, Vulnerability, Automated analysis, Satisfiability Modulo Theories (SMT), Dependency graph, Software development}
}
```

## A dataset on vulnerabilities affecting dependencies in software package managers

**Abstract:** The increasing reliance on third-party dependencies in software development introduces significant security risk challenges. This study presents a dataset that maps the vulnerabilities that affect dependencies in three major package managers: Node Package Manager (NPM), Python Package Index (PyPI), Cargo Crates and RubyGems. The dataset comprises information on 4437,679 unique packages and 60,950,846 versions of packages, with vulnerability data sourced from Open Source Vulnerabilities (OSV). It includes 270,430 known vulnerabilities linked to package versions, allowing a detailed analysis of security risks in software supply chains. Our methodology involved extracting dependency and version data from official package manager sources, correlating them with vulnerability reports, and storing the results in structured formats, including CSV and database dumps. The resultant dataset enables automated monitoring of vulnerable dependencies, facilitating analysis and security assessments, and defining mitigation strategies. This work identifies that 0.42 % of PyPI, 7.5 % of RubyGems, 3.91 % of Cargo and 6.93 % NPM versions rely on at least one vulnerable dependency. Furthermore, PyPI has 329 latest versions affected, RubyGem 919, Cargo 53, and NPM 14,858. This dataset provides valuable information for researchers, developers, and security professionals looking to improve software supply chain security. It provides a foundation for developing tools aimed at security and data analytics, enabling early vulnerability detection and improving mitigation controls for dependency-related security risks, thus promoting more secure software ecosystems. The dataset can be extended by incorporating additional packages, introducing new features, and ensuring continuous updates.

**Citation:**

```bibtex
@article{securechain-dib,
    title = {A dataset on vulnerabilities affecting dependencies in software package managers},
    journal = {Data in Brief},
    volume = {62},
    pages = {111903},
    year = {2025},
    issn = {2352-3409},
    doi = {https://doi.org/10.1016/j.dib.2025.111903},
    url = {https://www.sciencedirect.com/science/article/pii/S2352340925006274},
    author = {A. Germán Márquez and Ángel Jesús Varela-Vaca and María Teresa {Gómez López}},
    keywords = {Security, Vulnerability, PyPI, Package, RubyGems, Cargo, NPM}
}
```
