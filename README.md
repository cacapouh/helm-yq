https://github.com/helm/helm/issues/4680#issuecomment-1468229115

### yqのインストール

```shell
brew install yq
```

### helm template

```
$ helm template ingress-nginx/ingress-nginx | tail
        - ALL
      readOnlyRootFilesystem: true
      runAsNonRoot: true
      runAsUser: 65532
      seccompProfile:
        type: RuntimeDefault
restartPolicy: OnFailure
serviceAccountName: release-name-ingress-nginx-admission
nodeSelector: 
  kubernetes.io/os: linux

$ helm template ingress-nginx/ingress-nginx | wc -l
738
```

```
$ helm template ingress-nginx/ingress-nginx 
---
# Source: ingress-nginx/templates/controller-serviceaccount.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: controller
  name: release-name-ingress-nginx
  namespace: default
automountServiceAccountToken: true
---
# Source: ingress-nginx/templates/controller-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: controller
  name: release-name-ingress-nginx-controller
  namespace: default
data:
  allow-snippet-annotations: "false"
---
# Source: ingress-nginx/templates/clusterrole.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
  name: release-name-ingress-nginx
rules:
  - apiGroups:
      - ""
    resources:
      - configmaps
      - endpoints
      - nodes
      - pods
      - secrets
      - namespaces
    verbs:
      - list
      - watch
  - apiGroups:
      - coordination.k8s.io
    resources:
      - leases
    verbs:
      - list
      - watch
  - apiGroups:
      - ""
    resources:
      - nodes
    verbs:
      - get
  - apiGroups:
      - ""
    resources:
      - services
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - networking.k8s.io
    resources:
      - ingresses
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - ""
    resources:
      - events
    verbs:
      - create
      - patch
  - apiGroups:
      - networking.k8s.io
    resources:
      - ingresses/status
    verbs:
      - update
  - apiGroups:
      - networking.k8s.io
    resources:
      - ingressclasses
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - discovery.k8s.io
    resources:
      - endpointslices
    verbs:
      - list
      - watch
      - get
---
# Source: ingress-nginx/templates/clusterrolebinding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
  name: release-name-ingress-nginx
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: release-name-ingress-nginx
subjects:
  - kind: ServiceAccount
    name: release-name-ingress-nginx
    namespace: default
---
# Source: ingress-nginx/templates/controller-role.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: controller
  name: release-name-ingress-nginx
  namespace: default
rules:
  - apiGroups:
      - ""
    resources:
      - namespaces
    verbs:
      - get
  - apiGroups:
      - ""
    resources:
      - configmaps
      - pods
      - secrets
      - endpoints
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - ""
    resources:
      - services
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - networking.k8s.io
    resources:
      - ingresses
    verbs:
      - get
      - list
      - watch
  # Omit Ingress status permissions if `--update-status` is disabled.
  - apiGroups:
      - networking.k8s.io
    resources:
      - ingresses/status
    verbs:
      - update
  - apiGroups:
      - networking.k8s.io
    resources:
      - ingressclasses
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - coordination.k8s.io
    resources:
      - leases
    resourceNames:
      - release-name-ingress-nginx-leader
    verbs:
      - get
      - update
  - apiGroups:
      - coordination.k8s.io
    resources:
      - leases
    verbs:
      - create
  - apiGroups:
      - ""
    resources:
      - events
    verbs:
      - create
      - patch
  - apiGroups:
      - discovery.k8s.io
    resources:
      - endpointslices
    verbs:
      - list
      - watch
      - get
---
# Source: ingress-nginx/templates/controller-rolebinding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: controller
  name: release-name-ingress-nginx
  namespace: default
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: release-name-ingress-nginx
subjects:
  - kind: ServiceAccount
    name: release-name-ingress-nginx
    namespace: default
---
# Source: ingress-nginx/templates/controller-service-webhook.yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: controller
  name: release-name-ingress-nginx-controller-admission
  namespace: default
spec:
  type: ClusterIP
  ports:
    - name: https-webhook
      port: 443
      targetPort: webhook
      appProtocol: https
  selector:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/component: controller
---
# Source: ingress-nginx/templates/controller-service.yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: controller
  name: release-name-ingress-nginx-controller
  namespace: default
spec:
  type: LoadBalancer
  ipFamilyPolicy: SingleStack
  ipFamilies: 
    - IPv4
  ports:
    - name: http
      port: 80
      protocol: TCP
      targetPort: http
      appProtocol: http
    - name: https
      port: 443
      protocol: TCP
      targetPort: https
      appProtocol: https
  selector:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/component: controller
---
# Source: ingress-nginx/templates/controller-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: controller
  name: release-name-ingress-nginx-controller
  namespace: default
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: ingress-nginx
      app.kubernetes.io/instance: release-name
      app.kubernetes.io/component: controller
  replicas: 1
  revisionHistoryLimit: 10
  minReadySeconds: 0
  template:
    metadata:
      labels:
        helm.sh/chart: ingress-nginx-4.9.0
        app.kubernetes.io/name: ingress-nginx
        app.kubernetes.io/instance: release-name
        app.kubernetes.io/version: "1.9.5"
        app.kubernetes.io/part-of: ingress-nginx
        app.kubernetes.io/managed-by: Helm
        app.kubernetes.io/component: controller
    spec:
      dnsPolicy: ClusterFirst
      containers:
        - name: controller
          image: registry.k8s.io/ingress-nginx/controller:v1.9.5@sha256:b3aba22b1da80e7acfc52b115cae1d4c687172cbf2b742d5b502419c25ff340e
          imagePullPolicy: IfNotPresent
          lifecycle: 
            preStop:
              exec:
                command:
                - /wait-shutdown
          args: 
            - /nginx-ingress-controller
            - --publish-service=$(POD_NAMESPACE)/release-name-ingress-nginx-controller
            - --election-id=release-name-ingress-nginx-leader
            - --controller-class=k8s.io/ingress-nginx
            - --ingress-class=nginx
            - --configmap=$(POD_NAMESPACE)/release-name-ingress-nginx-controller
            - --validating-webhook=:8443
            - --validating-webhook-certificate=/usr/local/certificates/cert
            - --validating-webhook-key=/usr/local/certificates/key
          securityContext: 
            runAsNonRoot: true
            runAsUser: 101
            allowPrivilegeEscalation: false
            seccompProfile: 
              type: RuntimeDefault
            capabilities:
              drop:
              - ALL
              add:
              - NET_BIND_SERVICE
            readOnlyRootFilesystem: false
          env:
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
            - name: LD_PRELOAD
              value: /usr/local/lib/libmimalloc.so
          livenessProbe: 
            failureThreshold: 5
            httpGet:
              path: /healthz
              port: 10254
              scheme: HTTP
            initialDelaySeconds: 10
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 1
          readinessProbe: 
            failureThreshold: 3
            httpGet:
              path: /healthz
              port: 10254
              scheme: HTTP
            initialDelaySeconds: 10
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 1
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
            - name: https
              containerPort: 443
              protocol: TCP
            - name: webhook
              containerPort: 8443
              protocol: TCP
          volumeMounts:
            - name: webhook-cert
              mountPath: /usr/local/certificates/
              readOnly: true
          resources: 
            requests:
              cpu: 100m
              memory: 90Mi
      nodeSelector: 
        kubernetes.io/os: linux
      serviceAccountName: release-name-ingress-nginx
      terminationGracePeriodSeconds: 300
      volumes:
        - name: webhook-cert
          secret:
            secretName: release-name-ingress-nginx-admission
---
# Source: ingress-nginx/templates/controller-ingressclass.yaml
# We don't support namespaced ingressClass yet
# So a ClusterRole and a ClusterRoleBinding is required
apiVersion: networking.k8s.io/v1
kind: IngressClass
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: controller
  name: nginx
spec:
  controller: k8s.io/ingress-nginx
---
# Source: ingress-nginx/templates/admission-webhooks/validating-webhook.yaml
# before changing this value, check the required kubernetes version
# https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/#prerequisites
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  annotations:
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: admission-webhook
  name: release-name-ingress-nginx-admission
webhooks:
  - name: validate.nginx.ingress.kubernetes.io
    matchPolicy: Equivalent
    rules:
      - apiGroups:
          - networking.k8s.io
        apiVersions:
          - v1
        operations:
          - CREATE
          - UPDATE
        resources:
          - ingresses
    failurePolicy: Fail
    sideEffects: None
    admissionReviewVersions:
      - v1
    clientConfig:
      service:
        name: release-name-ingress-nginx-controller-admission
        namespace: default
        path: /networking/v1/ingresses
---
# Source: ingress-nginx/templates/admission-webhooks/job-patch/serviceaccount.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: release-name-ingress-nginx-admission
  namespace: default
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade,post-install,post-upgrade
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: admission-webhook
---
# Source: ingress-nginx/templates/admission-webhooks/job-patch/clusterrole.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: release-name-ingress-nginx-admission
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade,post-install,post-upgrade
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: admission-webhook
rules:
  - apiGroups:
      - admissionregistration.k8s.io
    resources:
      - validatingwebhookconfigurations
    verbs:
      - get
      - update
---
# Source: ingress-nginx/templates/admission-webhooks/job-patch/clusterrolebinding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: release-name-ingress-nginx-admission
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade,post-install,post-upgrade
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: admission-webhook
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: release-name-ingress-nginx-admission
subjects:
  - kind: ServiceAccount
    name: release-name-ingress-nginx-admission
    namespace: default
---
# Source: ingress-nginx/templates/admission-webhooks/job-patch/role.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: release-name-ingress-nginx-admission
  namespace: default
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade,post-install,post-upgrade
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: admission-webhook
rules:
  - apiGroups:
      - ""
    resources:
      - secrets
    verbs:
      - get
      - create
---
# Source: ingress-nginx/templates/admission-webhooks/job-patch/rolebinding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: release-name-ingress-nginx-admission
  namespace: default
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade,post-install,post-upgrade
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: admission-webhook
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: release-name-ingress-nginx-admission
subjects:
  - kind: ServiceAccount
    name: release-name-ingress-nginx-admission
    namespace: default
---
# Source: ingress-nginx/templates/admission-webhooks/job-patch/job-createSecret.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: release-name-ingress-nginx-admission-create
  namespace: default
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: admission-webhook
spec:
  template:
    metadata:
      name: release-name-ingress-nginx-admission-create
      labels:
        helm.sh/chart: ingress-nginx-4.9.0
        app.kubernetes.io/name: ingress-nginx
        app.kubernetes.io/instance: release-name
        app.kubernetes.io/version: "1.9.5"
        app.kubernetes.io/part-of: ingress-nginx
        app.kubernetes.io/managed-by: Helm
        app.kubernetes.io/component: admission-webhook
    spec:
      containers:
        - name: create
          image: registry.k8s.io/ingress-nginx/kube-webhook-certgen:v20231011-8b53cabe0@sha256:a7943503b45d552785aa3b5e457f169a5661fb94d82b8a3373bcd9ebaf9aac80
          imagePullPolicy: IfNotPresent
          args:
            - create
            - --host=release-name-ingress-nginx-controller-admission,release-name-ingress-nginx-controller-admission.$(POD_NAMESPACE).svc
            - --namespace=$(POD_NAMESPACE)
            - --secret-name=release-name-ingress-nginx-admission
          env:
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          securityContext: 
            allowPrivilegeEscalation: false
            capabilities:
              drop:
              - ALL
            readOnlyRootFilesystem: true
            runAsNonRoot: true
            runAsUser: 65532
            seccompProfile:
              type: RuntimeDefault
      restartPolicy: OnFailure
      serviceAccountName: release-name-ingress-nginx-admission
      nodeSelector: 
        kubernetes.io/os: linux
---
# Source: ingress-nginx/templates/admission-webhooks/job-patch/job-patchWebhook.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: release-name-ingress-nginx-admission-patch
  namespace: default
  annotations:
    "helm.sh/hook": post-install,post-upgrade
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
  labels:
    helm.sh/chart: ingress-nginx-4.9.0
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "1.9.5"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: admission-webhook
spec:
  template:
    metadata:
      name: release-name-ingress-nginx-admission-patch
      labels:
        helm.sh/chart: ingress-nginx-4.9.0
        app.kubernetes.io/name: ingress-nginx
        app.kubernetes.io/instance: release-name
        app.kubernetes.io/version: "1.9.5"
        app.kubernetes.io/part-of: ingress-nginx
        app.kubernetes.io/managed-by: Helm
        app.kubernetes.io/component: admission-webhook
    spec:
      containers:
        - name: patch
          image: registry.k8s.io/ingress-nginx/kube-webhook-certgen:v20231011-8b53cabe0@sha256:a7943503b45d552785aa3b5e457f169a5661fb94d82b8a3373bcd9ebaf9aac80
          imagePullPolicy: IfNotPresent
          args:
            - patch
            - --webhook-name=release-name-ingress-nginx-admission
            - --namespace=$(POD_NAMESPACE)
            - --patch-mutating=false
            - --secret-name=release-name-ingress-nginx-admission
            - --patch-failure-policy=Fail
          env:
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          securityContext: 
            allowPrivilegeEscalation: false
            capabilities:
              drop:
              - ALL
            readOnlyRootFilesystem: true
            runAsNonRoot: true
            runAsUser: 65532
            seccompProfile:
              type: RuntimeDefault
      restartPolicy: OnFailure
      serviceAccountName: release-name-ingress-nginx-admission
      nodeSelector: 
        kubernetes.io/os: linux
```

### helm template, yq


```
$ helm template ingress-nginx/ingress-nginx | yq -s '"prefix-" + $index'
$ ls
prefix-0.yml    prefix-10.yml   prefix-12.yml   prefix-14.yml   prefix-16.yml   prefix-2.yml    prefix-4.yml    prefix-6.yml    prefix-8.yml
prefix-1.yml    prefix-11.yml   prefix-13.yml   prefix-15.yml   prefix-17.yml   prefix-3.yml    prefix-5.yml    prefix-7.yml    prefix-9.yml
```


```
$ helm template ingress-nginx/ingress-nginx | yq -s '.kind + "-" + $index'
$ ls
ClusterRole-12.yml                      ConfigMap-1.yml                         Job-17.yml                              RoleBinding-5.yml                       ServiceAccount-11.yml
ClusterRole-2.yml                       Deployment-8.yml                        Role-14.yml                             Service-6.yml                           ValidatingWebhookConfiguration-10.yml
ClusterRoleBinding-13.yml               IngressClass-9.yml                      Role-4.yml                              Service-7.yml
ClusterRoleBinding-3.yml                Job-16.yml 
```
