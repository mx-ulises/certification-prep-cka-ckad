# Run Open Policy Agent's conftest static analisys

Links:
 - [Conftest installation and Docker](https://www.conftest.dev/install/)
 - [Conftest examples](https://www.conftest.dev/)

## Test kubernetes spec files

Link: [Examples](https://github.com/open-policy-agent/conftest/tree/master/examples/kubernetes)

  1. Create your tests in a `rego` file and save it in `policy` directory. Example in [`deployment.rego`](policy/deployment.rego).
  1. Run conftest as follow:
     ```
     # docker run --rm -v $(pwd):/project openpolicyagent/conftest test <kubernetes_spec_file>
     docker run --rm -v $(pwd):/project openpolicyagent/conftest test deployment-spec.yaml
     ```

## Test Dockerfiles

Link: [Examples](https://github.com/open-policy-agent/conftest/tree/master/examples/docker)

  1. Create your tests in a `rego` file and save it in `policy` directory. Example in [`commands.rego`](policy/commands.rego).
  1. Run conftest as follow:
     ```
     # docker run --rm -v $(pwd):/project openpolicyagent/conftest test <dockerfile> --all-namespaces
     docker run --rm -v $(pwd):/project openpolicyagent/conftest test Dockerfile --all-namespaces
     ```
