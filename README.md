# court-examples

A collection of hands-on examples to explore and experiment with [Court](https://github.com/ghdrope/court).

## Examples

Examples follow an incremental naming convention aligned with the evolution of the project.

Each example should:

- Live in its own directory
- Be self-contained
- Include a `README.md` with setup and steps to follow

When contributing, create a new directory for the example and include clear instructions for experimentation.

---

## Getting Started

1. Create namespace

    ```bash
    kubectl create namespace court
    ```

2. Create VCS token secret

    Court creates GitHub issues for detected incidents.
    You need a GitHub token with issues read/write permissions:

    ```bash
    kubectl create secret generic vcs-tokens \
      --from-literal=GITHUB_TOKEN=<github_pat_...> \
    -n court
    ```

3. Install Court via Helm

    ```bash
    helm repo add court https://ghdrope.github.io/court
    helm repo update
    helm install court court/court --namespace court
    ```

---

## Usage

After installation, each example in this repository can be applied independently.

Follow the instructions inside each example directory.

---

## Contributing

Feedback is welcome via Issues.

New examples are also very welcome, feel free to open a PR.

Any contribution helps improve the project.

---

## License

Licensed under the Apache License 2.0.

---

Thank you ! 👩‍⚖️👨‍⚖️
