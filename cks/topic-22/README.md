# Open Policy Agent

Links:
 - [Open Policy Agent gatekeeper](https://kubernetes.io/blog/2019/08/06/opa-gatekeeper-policy-and-governance-for-kubernetes/).
 - [Examples in github](https://github.com/open-policy-agent/gatekeeper/tree/master/demo)

## Install OPA Gatekeeper

The cluster should have installed OPA Gatekeeper, that can be done following the [`installation instructions`](https://open-policy-agent.github.io/gatekeeper/website/docs/install/)

```
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/v3.16.0/deploy/gatekeeper.yaml
```

## Create a constraint template

The OPA Gatekeeper `ConstraintTemplate` are specifications of test that would be running against the specs of some resources in kubernetes that will prevent their creation if they fail the test. The specifications are written as Kubernetes CRD and the test is written in rego. This is an example of [ct-k8srequiredlabels.yaml](ct-k8srequiredlabels.yaml):

```
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: k8srequiredlabels
spec:
  crd:
    spec:
      names:
        kind: K8sRequiredLabels
      validation:
        # Schema for the `parameters` field
        openAPIV3Schema:
          type: object
          properties:
            labels:
              type: array
              items:
                type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8srequiredlabels

        violation[{"msg": msg, "details": {"missing_labels": missing}}] {
          provided := {label | input.review.object.metadata.labels[label]}
          required := {label | label := input.parameters.labels[_]}
          missing := required - provided
          count(missing) > 0
          msg := sprintf("you must provide labels: %v", [missing])
        }
```

## Create a Constraint

Once you have a template created, you can create a `Constraint` resource of the type of your template with different parameters, and it will enforce it:

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: ns-must-have-gk-dryrun
spec:
  enforcementAction: dryrun
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Namespace"]
  parameters:
    labels: ["hr"]

```
