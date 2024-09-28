# Kubernetes HPA 架構說明

## 架構圖

![架構圖](architecture-diagram.png)

## 操作方法

### 部署應用程式

1. 創建 `example-deployment.yaml` 文件：

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: example-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: example
      template:
        metadata:
          labels:
            app: example
        spec:
          nodeSelector:
            node-role.kubernetes.io/application: "true"
          containers:
          - name: example
            image: nginx
            ports:
            - containerPort: 80
    ```

2. 使用 `kubectl` 命令來部署應用程式：

    ```sh
    kubectl apply -f example-deployment.yaml
    ```

### 配置 HPA

1. 創建 `hpa.yaml` 文件：

    ```yaml
    apiVersion: autoscaling/v1
    kind: HorizontalPodAutoscaler
    metadata:
      name: example-hpa
    spec:
      scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment
        name: example-deployment
      minReplicas: 1
      maxReplicas: 10
      targetCPUUtilizationPercentage: 50
    ```

2. 使用 `kubectl` 命令來應用 HPA 配置：

    ```sh
    kubectl apply -f hpa.yaml
    ```

## 使用到的配置檔

- `example-deployment.yaml`: 部署應用程式的配置檔
- `hpa.yaml`: 配置 HPA 的配置檔
