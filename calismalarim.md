# Çalışmalarım

## minikube start
minikube başlatma komutu

## minikube status
minikube kontrol komutu

```
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

## kubectl get pods
podları listeler

```
NAME                                                READY   STATUS    RESTARTS      AGE
my-nginx-nginx-ingress-controller-5c64cd794-k4kzq   1/1     Running   8 (89m ago)   11d

```

## kubectl get deployments -o yaml -n kube-system

```
apiVersion: v1
items:
- apiVersion: apps/v1
  kind: Deployment
  metadata:
    annotations:
      deployment.kubernetes.io/revision: "1"
    creationTimestamp: "2024-10-11T08:30:33Z"
    generation: 2
    labels:
      k8s-app: kube-dns
    name: coredns
    namespace: kube-system
    resourceVersion: "24317"
    uid: 66dc3a44-9dc1-4334-98c8-96ef4116b266
  spec:
    progressDeadlineSeconds: 600
    replicas: 1
    revisionHistoryLimit: 10
    selector:
      matchLabels:
        k8s-app: kube-dns
    strategy:
      rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 1
      type: RollingUpdate
    template:
      metadata:
        creationTimestamp: null
        labels:
          k8s-app: kube-dns
      spec:
        affinity:
          podAntiAffinity:
            preferredDuringSchedulingIgnoredDuringExecution:
            - podAffinityTerm:
                labelSelector:
                  matchExpressions:
                  - key: k8s-app
                    operator: In
                    values:
                    - kube-dns
                topologyKey: kubernetes.io/hostname
              weight: 100
        containers:
        - args:
          - -conf
          - /etc/coredns/Corefile
          image: registry.k8s.io/coredns/coredns:v1.11.1
          imagePullPolicy: IfNotPresent
          livenessProbe:
            failureThreshold: 5
            httpGet:
              path: /health
              port: 8080
              scheme: HTTP
            initialDelaySeconds: 60
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 5
          name: coredns
          ports:
          - containerPort: 53
            name: dns
            protocol: UDP
          - containerPort: 53
            name: dns-tcp
            protocol: TCP
          - containerPort: 9153
            name: metrics
            protocol: TCP
          readinessProbe:
            failureThreshold: 3
            httpGet:
              path: /ready
              port: 8181
              scheme: HTTP
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 1
          resources:
            limits:
              memory: 170Mi
            requests:
              cpu: 100m
              memory: 70Mi
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              add:
              - NET_BIND_SERVICE
              drop:
              - ALL
            readOnlyRootFilesystem: true
          terminationMessagePath: /dev/termination-log
          terminationMessagePolicy: File
          volumeMounts:
          - mountPath: /etc/coredns
            name: config-volume
            readOnly: true
        dnsPolicy: Default
        nodeSelector:
          kubernetes.io/os: linux
        priorityClassName: system-cluster-critical
        restartPolicy: Always
        schedulerName: default-scheduler
        securityContext: {}
        serviceAccount: coredns
        serviceAccountName: coredns
        terminationGracePeriodSeconds: 30
        tolerations:
        - key: CriticalAddonsOnly
          operator: Exists
        - effect: NoSchedule
          key: node-role.kubernetes.io/control-plane
        volumes:
        - configMap:
            defaultMode: 420
            items:
            - key: Corefile
              path: Corefile
            name: coredns
          name: config-volume
  status:
    availableReplicas: 1
    conditions:
    - lastTransitionTime: "2024-10-11T08:30:39Z"
      lastUpdateTime: "2024-10-11T08:30:39Z"
      message: Deployment has minimum availability.
      reason: MinimumReplicasAvailable
      status: "True"
      type: Available
    - lastTransitionTime: "2024-10-11T08:30:39Z"
      lastUpdateTime: "2024-10-11T08:31:14Z"
      message: ReplicaSet "coredns-6f6b679f8f" has successfully progressed.
      reason: NewReplicaSetAvailable
      status: "True"
      type: Progressing
    observedGeneration: 2
    readyReplicas: 1
    replicas: 1
    updatedReplicas: 1
kind: List
metadata:
  resourceVersion: ""
  ```


## kubectl get deployments -o json -n kube-system


  ```
  {
    "apiVersion": "v1",
    "items": [
        {
            "apiVersion": "apps/v1",
            "kind": "Deployment",
            "metadata": {
                "annotations": {
                    "deployment.kubernetes.io/revision": "1"
                },
                "creationTimestamp": "2024-10-11T08:30:33Z",
                "generation": 2,
                "labels": {
                    "k8s-app": "kube-dns"
                },
                "name": "coredns",
                "namespace": "kube-system",
                "resourceVersion": "24317",
                "uid": "66dc3a44-9dc1-4334-98c8-96ef4116b266"
            },
            "spec": {
                "progressDeadlineSeconds": 600,
                "replicas": 1,
                "revisionHistoryLimit": 10,
                "selector": {
                    "matchLabels": {
                        "k8s-app": "kube-dns"
                    }
                },
                "strategy": {
                    "rollingUpdate": {
                        "maxSurge": "25%",
                        "maxUnavailable": 1
                    },
                    "type": "RollingUpdate"
                },
                "template": {
                    "metadata": {
                        "creationTimestamp": null,
                        "labels": {
                            "k8s-app": "kube-dns"
                        }
                    },
                    "spec": {
                        "affinity": {
                            "podAntiAffinity": {
                                "preferredDuringSchedulingIgnoredDuringExecution": [
                                    {
                                        "podAffinityTerm": {
                                            "labelSelector": {
                                                "matchExpressions": [
                                                    {
                                                        "key": "k8s-app",
                                                        "operator": "In",
                                                        "values": [
                                                            "kube-dns"
                                                        ]
                                                    }
                                                ]
                                            },
                                            "topologyKey": "kubernetes.io/hostname"
                                        },
                                        "weight": 100
                                    }
                                ]
                            }
                        },
                        "containers": [
                            {
                                "args": [
                                    "-conf",
                                    "/etc/coredns/Corefile"
                                ],
                                "image": "registry.k8s.io/coredns/coredns:v1.11.1",
                                "imagePullPolicy": "IfNotPresent",
                                "livenessProbe": {
                                    "failureThreshold": 5,
                                    "httpGet": {
                                        "path": "/health",
                                        "port": 8080,
                                        "scheme": "HTTP"
                                    },
                                    "initialDelaySeconds": 60,
                                    "periodSeconds": 10,
                                    "successThreshold": 1,
                                    "timeoutSeconds": 5
                                },
                                "name": "coredns",
                                "ports": [
                                    {
                                        "containerPort": 53,
                                        "name": "dns",
                                        "protocol": "UDP"
                                    },
                                    {
                                        "containerPort": 53,
                                        "name": "dns-tcp",
                                        "protocol": "TCP"
                                    },
                                    {
                                        "containerPort": 9153,
                                        "name": "metrics",
                                        "protocol": "TCP"
                                    }
                                ],
                                "readinessProbe": {
                                    "failureThreshold": 3,
                                    "httpGet": {
                                        "path": "/ready",
                                        "port": 8181,
                                        "scheme": "HTTP"
                                    },
                                    "periodSeconds": 10,
                                    "successThreshold": 1,
                                    "timeoutSeconds": 1
                                },
                                "resources": {
                                    "limits": {
                                        "memory": "170Mi"
                                    },
                                    "requests": {
                                        "cpu": "100m",
                                        "memory": "70Mi"
                                    }
                                },
                                "securityContext": {
                                    "allowPrivilegeEscalation": false,
                                    "capabilities": {
                                        "add": [
                                            "NET_BIND_SERVICE"
                                        ],
                                        "drop": [
                                            "ALL"
                                        ]
                                    },
                                    "readOnlyRootFilesystem": true
                                },
                                "terminationMessagePath": "/dev/termination-log",
                                "terminationMessagePolicy": "File",
                                "volumeMounts": [
                                    {
                                        "mountPath": "/etc/coredns",
                                        "name": "config-volume",
                                        "readOnly": true
                                    }
                                ]
                            }
                        ],
                        "dnsPolicy": "Default",
                        "nodeSelector": {
                            "kubernetes.io/os": "linux"
                        },
                        "priorityClassName": "system-cluster-critical",
                        "restartPolicy": "Always",
                        "schedulerName": "default-scheduler",
                        "securityContext": {},
                        "serviceAccount": "coredns",
                        "serviceAccountName": "coredns",
                        "terminationGracePeriodSeconds": 30,
                        "tolerations": [
                            {
                                "key": "CriticalAddonsOnly",
                                "operator": "Exists"
                            },
                            {
                                "effect": "NoSchedule",
                                "key": "node-role.kubernetes.io/control-plane"
                            }
                        ],
                        "volumes": [
                            {
                                "configMap": {
                                    "defaultMode": 420,
                                    "items": [
                                        {
                                            "key": "Corefile",
                                            "path": "Corefile"
                                        }
                                    ],
                                    "name": "coredns"
                                },
                                "name": "config-volume"
                            }
                        ]
                    }
                }
            },
            "status": {
                "availableReplicas": 1,
                "conditions": [
                    {
                        "lastTransitionTime": "2024-10-11T08:30:39Z",
                        "lastUpdateTime": "2024-10-11T08:30:39Z",
                        "message": "Deployment has minimum availability.",
                        "reason": "MinimumReplicasAvailable",
                        "status": "True",
                        "type": "Available"
                    },
                    {
                        "lastTransitionTime": "2024-10-11T08:30:39Z",
                        "lastUpdateTime": "2024-10-11T08:31:14Z",
                        "message": "ReplicaSet \"coredns-6f6b679f8f\" has successfully progressed.",
                        "reason": "NewReplicaSetAvailable",
                        "status": "True",
                        "type": "Progressing"
                    }
                ],
                "observedGeneration": 2,
                "readyReplicas": 1,
                "replicas": 1,
                "updatedReplicas": 1
            }
        }
    ],
    "kind": "List",
    "metadata": {
        "resourceVersion": ""
    }
}
```