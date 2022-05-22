---
title: Vertical Pod Scaling
summary: Vertical Pod Scaling.
authors:
    - Levent Ogut
date: 2022-05-12-12:06
tags:
    - Kubernetes
    - Scaling
    - vertical pod scaling
    - pod scaling
---
## Vertical pod scaling (VPA) configuration

```yaml
# vpa.yaml
---
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: test-vpa
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind:       Deployment
    name:       test-deployment
  updatePolicy:
    updateMode: "Auto"
```

[updateMode](https://github.com/kubernetes/autoscaler/blob/128bfa5d435cd34380d5650202dc66b417f3f11d/vertical-pod-autoscaler/pkg/apis/autoscaling.k8s.io/v1/types.go#L122) is crucial here, it's recommended to deploy with update mode "Off."

```golang
// UpdateMode controls when autoscaler applies changes to the pod resoures.
// +kubebuilder:validation:Enum=Off;Initial;Recreate;Auto
type UpdateMode string

const (
	// UpdateModeOff means that autoscaler never changes Pod resources.
	// The recommender still sets the recommended resources in the
	// VerticalPodAutoscaler object. This can be used for a "dry run".
	UpdateModeOff UpdateMode = "Off"
	// UpdateModeInitial means that autoscaler only assigns resources on pod
	// creation and does not change them during the lifetime of the pod.
	UpdateModeInitial UpdateMode = "Initial"
	// UpdateModeRecreate means that autoscaler assigns resources on pod
	// creation and additionally can update them during the lifetime of the
	// pod by deleting and recreating the pod.
	UpdateModeRecreate UpdateMode = "Recreate"
	// UpdateModeAuto means that autoscaler assigns resources on pod creation
	// and additionally can update them during the lifetime of the pod,
	// using any available update method. Currently this is equivalent to
	// Recreate, which is the only available update method.
	UpdateModeAuto UpdateMode = "Auto"
)
```

## Get output

```yaml
$ kubectl get vpa test-vpa

NAME                  MODE   CPU   MEM        PROVIDED   AGE
test-vpa              Auto   1m    10485760   True       26h
```

## YAML output

```shell
kubectl get vpa test-vpa -o yaml
```

```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  annotations:
  creationTimestamp: "2022-03-25T05:44:02Z"
  generation: 692
  name: test-vpa
  namespace: default
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: test-deployment
  updatePolicy:
    updateMode: Auto
status:
  conditions:
  - lastTransitionTime: "2022-03-25T05:44:34Z"
    status: "False"
    type: LowConfidence
  - lastTransitionTime: "2022-03-25T05:44:34Z"
    status: "True"
    type: RecommendationProvided
  recommendation:
    containerRecommendations:
    - containerName: sidecar
      lowerBound:
        cpu: 1m
        memory: "9437184"
      target:
        cpu: 1m
        memory: "10485760"
      uncappedTarget:
        cpu: 1m
        memory: "10485760"
      upperBound:
        cpu: 2m
        memory: "12582912"
    - containerName: test-deployment
      lowerBound:
        cpu: 5m
        memory: "179306496"
      target:
        cpu: 6m
        memory: "243269632"
      uncappedTarget:
        cpu: 6m
        memory: "243269632"
      upperBound:
        cpu: 7m
        memory: "273678336"
```

## References & further reading

- [Kubernetes Repository](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)

- [Google Kubernetes Engine (GKE) >
Documentation >
Guides > Vertical Pod autoscaling](https://cloud.google.com/kubernetes-engine/docs/concepts/verticalpodautoscaler)

- [AWS EKS > User Guide > Vertical Pod Autoscaler](https://docs.aws.amazon.com/eks/latest/userguide/vertical-pod-autoscaler.html)
