---
title: "Blog 3"
date: "`r Sys.Date()`"
weight: 303
chapter: false
pre: " <b> 3.3. </b> "
---

# Triển khai LLMs trên Amazon EKS sử dụng vLLM Deep Learning Containers

*Tác giả: Vishal Naik và Sumeet Tripathi vào ngày 14 AUG 2025 trong [AWS Deep Learning AMIs](https://aws.amazon.com/blogs/architecture/category/artificial-intelligence/aws-deep-learning-amis/), [Best Practices](https://aws.amazon.com/blogs/architecture/category/post-types/best-practices/), [Expert (400)](https://aws.amazon.com/blogs/architecture/category/learning-levels/expert-400/), [Generative AI](https://aws.amazon.com/blogs/architecture/category/artificial-intelligence/generative-ai/), [Technical How-to](https://aws.amazon.com/blogs/architecture/category/post-types/technical-how-to/)*

Các tổ chức đối mặt với những thách thức đáng kể khi triển khai các mô hình ngôn ngữ lớn (LLMs) một cách hiệu quả ở quy mô lớn. Những thách thức chính bao gồm tối ưu hóa việc sử dụng tài nguyên GPU, quản lý hạ tầng mạng, và cung cấp quyền truy cập hiệu quả đến các trọng số mô hình. Khi chạy các khối lượng công việc suy luận phân tán, các tổ chức thường gặp phải sự phức tạp trong việc điều phối các hoạt động mô hình trên nhiều nút. Những thách thức phổ biến bao gồm phân phối hiệu quả các thành phần mô hình trên các GPU có sẵn, phối hợp giao tiếp liền mạch giữa các đơn vị xử lý, và duy trì hiệu suất nhất quán với độ trễ thấp và thông lượng cao.

[vLLM](https://docs.vllm.ai/en/latest/) là một thư viện mã nguồn mở cho suy luận và phục vụ LLM nhanh chóng. [vLLM AWS Deep Learning Containers (DLCs)](https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/dlc-vllm-0-8-x86-ec2.html) được tối ưu hóa cho khách hàng triển khai vLLMs trên [Amazon Elastic Compute Cloud](http://aws.amazon.com/ec2) (Amazon EC2), [Amazon Elastic Container Service](http://aws.amazon.com/ecs) (Amazon ECS), và [Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (Amazon EKS), và được cung cấp miễn phí. Những container này đóng gói một môi trường được cấu hình trước, đã kiểm tra hoạt động liền mạch ngay từ đầu, bao gồm các phụ thuộc cần thiết như driver và thư viện để chạy vLLMs hiệu quả, và cung cấp hỗ trợ tích hợp cho [Elastic Fabric Adapter](https://aws.amazon.com/hpc/efa/) (EFA) cho các khối lượng công việc suy luận đa nút hiệu suất cao. Bạn không cần phải xây dựng môi trường suy luận từ đầu nữa. Thay vào đó, bạn có thể cài đặt vLLM DLC và nó sẽ tự động thiết lập và cấu hình môi trường, và bạn có thể bắt đầu triển khai các khối lượng công việc suy luận ở quy mô lớn.

Trong bài viết này, chúng tôi trình bày cách triển khai mô hình DeepSeek-R1-Distill-Qwen-32B sử dụng AWS DLCs cho vLLMs trên Amazon EKS, thể hiện cách những container được xây dựng có mục đích này đơn giản hóa việc triển khai engine suy luận mã nguồn mở mạnh mẽ này. Giải pháp này có thể giúp bạn giải quyết các thách thức hạ tầng phức tạp của việc triển khai LLMs trong khi vẫn duy trì hiệu suất và hiệu quả chi phí.

## AWS DLCs

AWS DLCs cung cấp cho các chuyên gia AI tạo sinh những môi trường Docker được tối ưu hóa để huấn luyện và triển khai các mô hình AI tạo sinh trong các pipeline và quy trình làm việc của họ trên Amazon EC2, Amazon EKS, và Amazon ECS. AWS DLCs được nhắm mục tiêu cho các khách hàng học máy (ML) tự quản lý muốn xây dựng và duy trì môi trường AI/ML của riêng họ, muốn kiểm soát cấp độ instance hạ tầng của họ, và quản lý các khối lượng công việc huấn luyện và suy luận của riêng họ. DLCs có sẵn dưới dạng Docker images cho huấn luyện và suy luận, và cũng với PyTorch và TensorFlow. DLCs được cập nhật với phiên bản mới nhất của frameworks và drivers, được kiểm tra tính tương thích và bảo mật, và được cung cấp miễn phí. Chúng cũng có thể được tùy chỉnh nhanh chóng bằng cách làm theo các hướng dẫn công thức của chúng tôi. Sử dụng AWS DLCs như một khối xây dựng cho môi trường AI tạo sinh giảm gánh nặng cho các nhóm vận hành và hạ tầng, giảm TCO cho hạ tầng AI/ML, tăng tốc phát triển sản phẩm AI tạo sinh, và giúp các nhóm AI tạo sinh tập trung vào công việc tạo giá trị gia tăng để thu được những hiểu biết được cung cấp bởi AI tạo sinh từ dữ liệu của tổ chức.

## Tổng quan giải pháp

Sơ đồ sau đây cho thấy sự tương tác giữa Amazon EKS, các instance EC2 hỗ trợ GPU với mạng EFA, và lưu trữ [Amazon FSx for Lustre](https://aws.amazon.com/fsx/lustre/). Các yêu cầu từ client chảy qua Application Load Balancer (ALB) đến các pod máy chủ vLLM chạy trên các nút EKS, truy cập các trọng số mô hình được lưu trữ trên FSx for Lustre. Kiến trúc này cung cấp một giải pháp có thể mở rộng, hiệu suất cao cho việc phục vụ các khối lượng công việc suy luận LLM với hiệu quả chi phí tối ưu.

![Kiến trúc giải pháp](/images/3-blog/3-image/image-1-4.png)

Sơ đồ sau đây minh họa ngăn xếp DLC trên AWS. Ngăn xếp này trình bày một kiến trúc toàn diện từ nền tảng instance EC2 thông qua container runtime, các driver GPU thiết yếu, và các framework ML như PyTorch. Sơ đồ phân tầng cho thấy cách CUDA, NCCL, và các thành phần quan trọng khác tích hợp để hỗ trợ các khối lượng công việc deep learning hiệu suất cao.

![Ngăn xếp DLC](/images/3-blog/3-image/image-2-3.png)

vLLM DLCs được tối ưu hóa đặc biệt cho suy luận hiệu suất cao, với hỗ trợ tích hợp cho tensor parallelism và pipeline parallelism trên nhiều GPU và nút. Tối ưu hóa này cho phép mở rộng hiệu quả các mô hình lớn như DeepSeek-R1-Distill-Qwen-32B, điều này sẽ thách thức để triển khai và quản lý. Các container cũng bao gồm cấu hình CUDA tối ưu và driver EFA, tạo điều kiện cho thông lượng tối đa cho các khối lượng công việc suy luận phân tán. Giải pháp này sử dụng các dịch vụ và thành phần AWS sau:

* **AWS DLCs cho vLLMs** – Docker images được cấu hình trước, tối ưu hóa để đơn giản hóa triển khai và tối đa hóa hiệu suất
* **Cụm EKS** – Cung cấp control plane Kubernetes để điều phối containers
* **Instances P4d.24xlarge** – [EC2 P4d instances](https://aws.amazon.com/ec2/instance-types/p4/) với 8 NVIDIA A100 GPUs mỗi instance, được cấu hình trong một managed node group
* **Elastic Fabric Adapter** – Giao diện mạng cho phép các ứng dụng điện toán hiệu suất cao mở rộng hiệu quả
* **FSx for Lustre** – Hệ thống file hiệu suất cao để lưu trữ trọng số mô hình
* **Mẫu LeaderWorkerSet** – Tài nguyên Kubernetes tùy chỉnh để triển khai vLLM trong cấu hình phân tán
* **AWS Load Balancer Controller** – Quản lý ALB cho truy cập bên ngoài

Bằng cách kết hợp những thành phần này, chúng ta tạo ra một hệ thống suy luận cung cấp khả năng phục vụ LLM độ trễ thấp, thông lượng cao với overhead vận hành tối thiểu.

## Điều kiện tiên quyết

Trước khi bắt đầu, hãy đảm bảo bạn có các điều kiện tiên quyết sau:

* Một [tài khoản AWS](https://portal.aws.amazon.com/billing/signup#/start/email) với quyền truy cập vào instances EC2 P4 (bạn có thể cần [yêu cầu tăng quota](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html))
* Quyền truy cập vào terminal có các công cụ sau được cài đặt:
  * [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) phiên bản 2.11.0 hoặc mới hơn
  * [eksctl](https://eksctl.io/installation/) phiên bản 0.150.0 hoặc mới hơn
  * [kubectl](https://kubernetes.io/docs/tasks/tools/) phiên bản 1.27 hoặc mới hơn
  * [Helm](https://helm.sh/docs/intro/install/) phiên bản 3.12.0 hoặc mới hơn
* Một [AWS CLI profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) (vllm-profile) được cấu hình với [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) role hoặc user có các quyền sau:
  * Tạo, quản lý, và xóa cụm EKS và node groups
  * Tạo, quản lý, và xóa tài nguyên EC2, bao gồm virtual private clouds (VPCs), subnets, security groups, và internet gateways
  * Tạo và quản lý IAM roles
  * Tạo, cập nhật, và xóa [AWS CloudFormation](http://aws.amazon.com/cloudformation) stacks
  * Tạo, xóa, và mô tả FSx file systems
  * Tạo và quản lý Elastic Load Balancers

Giải pháp này có thể được triển khai trong các AWS Regions nơi Amazon EKS, instances P4d, và FSx for Lustre có sẵn. Hướng dẫn này sử dụng Region us-west-2. Quá trình triển khai hoàn chỉnh mất khoảng 60–90 phút.

Clone repository GitHub của chúng tôi chứa các file cấu hình cần thiết:

```bash
# Clone repository
git clone https://github.com/aws-samples/sample-aws-deep-learning-containers.git
cd vllm-samples/deepseek/eks
```

## Tạo cụm EKS

Đầu tiên, chúng ta tạo một cụm EKS trong Region us-west-2 sử dụng file cấu hình được cung cấp. Điều này thiết lập Kubernetes control plane sẽ điều phối các container của chúng ta. Cụm được cấu hình với VPC, subnets, và security groups được tối ưu hóa để chạy các khối lượng công việc GPU.

```bash
# Cập nhật region trong eks-cluster.yaml nếu cần
sed -i "s|region: us-east-1|region: us-west-2|g" eks-cluster.yaml

# Tạo cụm EKS
eksctl create cluster -f eks-cluster.yaml --profile vllm-profile
```

Điều này sẽ mất khoảng 15–20 phút để hoàn thành. Trong thời gian này, eksctl tạo một CloudFormation stack cung cấp các tài nguyên cần thiết cho cụm EKS của bạn.

Bạn có thể xác thực việc tạo cụm với đoạn code sau:
![](/images/3-blog/3-image/image-3-2.png)

```bash
# Xác minh việc tạo cụm
eksctl get cluster --profile vllm-profile
# Kết quả mong đợi:
# NAME            REGION          EKSCTL CREATED
# vllm-cluster    us-west-2       True
```
Bạn cũng có thể tạo trên Amazon EKS console:
![](/images/3-blog/3-image/image-4-2.png)

## Tạo node group với hỗ trợ EFA

Tiếp theo, chúng ta tạo một managed node group với instances P4d.24xlarge có EFA được bật. Những instance này được trang bị 8 NVIDIA A100 GPUs mỗi instance, cung cấp sức mạnh tính toán đáng kể cho suy luận LLM.

```bash
# Lấy VPC ID từ cụm EKS
VPC_ID=$(aws --profile vllm-profile eks describe-cluster --name vllm-cluster \
  --query "cluster.resourcesVpcConfig.vpcId" --output text)

# Tìm availability zone của một private subnet
PRIVATE_AZ=$(aws --profile vllm-profile ec2 describe-subnets \
  --filters "Name=vpc-id,Values=$VPC_ID" "Name=map-public-ip-on-launch,Values=false" \
  --query "Subnets[0].AvailabilityZone" --output text)
echo "Selected private subnet AZ: $PRIVATE_AZ"

# Cập nhật phần nodegroup_az với giá trị private AZ
sed -i "s|availabilityZones: \[nodegroup_az\]|availabilityZones: [\"$PRIVATE_AZ\"]|g" large-model-nodegroup.yaml

# Xác minh thay đổi
grep "availabilityZones" large-model-nodegroup.yaml

# Tạo node group với hỗ trợ EFA
eksctl create nodegroup -f large-model-nodegroup.yaml --profile vllm-profile
```

Điều này sẽ mất khoảng 10–15 phút để hoàn thành. Sau khi node group được tạo, cấu hình kubectl để kết nối với cụm:

```bash
# Cấu hình kubectl để kết nối với cụm
aws eks update-kubeconfig --name vllm-cluster --region us-west-2 --profile vllm-profile

# Kiểm tra trạng thái node
kubectl get nodes

#Sau đây là một ví dụ về kết quả đầu ra mong đợi:
NAME                                            STATUS   ROLES    AGE     VERSION
ip-192-168-xx-xx.us-west-2.compute.internal     Ready    <none>   5m      v1.31.7-eks-xxxx
ip-192-168-yy-yy.us-west-2.compute.internal     Ready    <none>   5m      v1.31.7-eks-xxxx
```
Bạn cũng có thể xem các nhóm node đã được tạo trên giao diện điều khiển của Amazon EKS.
![](/images/3-blog/3-image/image-5-3.png)

## Kiểm tra NVIDIA device pods

Vì chúng ta đang sử dụng Amazon EKS optimized AMI với hỗ trợ GPU, NVIDIA device plugin đã được bao gồm trong cụm. Xác minh rằng NVIDIA device plugin đang chạy:

```bash
# Kiểm tra NVIDIA device plugin pods
kubectl get pods -n kube-system | grep nvidia

# Xác minh GPUs có sẵn trong cụm
kubectl get nodes -o json | jq '.items[].status.capacity."nvidia.com/gpu"'

#kết quả đầu ra mong đợi:
"8"
"8"
```

## Tạo hệ thống file FSx for Lustre

Để có hiệu suất tối ưu, chúng ta tạo một hệ thống file FSx for Lustre để lưu trữ trọng số mô hình của chúng ta:

```bash
# Tạo security group cho FSx Lustre
FSX_SG_ID=$(aws --profile vllm-profile ec2 create-security-group --group-name fsx-lustre-sg \
  --description "Security group for FSx Lustre" \
  --vpc-id $(aws --profile vllm-profile eks describe-cluster --name vllm-cluster \
  --query "cluster.resourcesVpcConfig.vpcId" --output text) \
  --query "GroupId" --output text)

echo "Created security group: $FSX_SG_ID"

# Thêm inbound rules cho FSx Lustre
aws --profile vllm-profile ec2 authorize-security-group-ingress --group-id $FSX_SG_ID \
  --protocol tcp --port 988-1023 \
  --source-group $(aws --profile vllm-profile eks describe-cluster --name vllm-cluster \
  --query "cluster.resourcesVpcConfig.clusterSecurityGroupId" --output text)

# Tạo FSx Lustre filesystem
SUBNET_ID=$(aws --profile vllm-profile eks describe-cluster --name vllm-cluster \
  --query "cluster.resourcesVpcConfig.subnetIds[0]" --output text)

FSX_ID=$(aws --profile vllm-profile fsx create-file-system --file-system-type LUSTRE \
  --storage-capacity 1200 --subnet-ids $SUBNET_ID \
  --security-group-ids $FSX_SG_ID --lustre-configuration DeploymentType=SCRATCH_2 \
  --tags Key=Name,Value=vllm-model-storage \
  --query "FileSystem.FileSystemId" --output text)

echo "Created FSx filesystem: $FSX_ID"
```
Hệ thống tệp được cấu hình với dung lượng lưu trữ 1,2 TB, loại triển khai là SCRATCH_2 cho hiệu năng cao, và các nhóm bảo mật (security groups) cho phép truy cập từ các nút EKS của chúng ta. Bạn cũng có thể kiểm tra hệ thống tệp FSx for Lustre trên bảng điều khiển của FSx for Lustre
![](/images/3-blog/3-image/image-6.png))
## Cài đặt AWS FSx CSI Driver

Để mount hệ thống file FSx for Lustre trong Kubernetes pods của chúng ta, chúng ta cài đặt AWS FSx CSI Driver:

```bash
# Thêm AWS FSx CSI Driver Helm repository
helm repo add aws-fsx-csi-driver https://kubernetes-sigs.github.io/aws-fsx-csi-driver/
helm repo update

# Cài đặt AWS FSx CSI Driver
helm install aws-fsx-csi-driver aws-fsx-csi-driver/aws-fsx-csi-driver --namespace kube-system

# Kiểm tra AWS FSx CSI Driver pods
kubectl get pods -n kube-system | grep fsx
```

## Tạo tài nguyên Kubernetes cho FSx for Lustre

Chúng ta tạo các tài nguyên Kubernetes cần thiết để sử dụng hệ thống file FSx for Lustre:

```bash
# Cập nhật storage class với subnet và security group IDs
sed -i "s|<subnet-id>|$SUBNET_ID|g" fsx-storage-class.yaml
sed -i "s|<sg-id>|$FSX_SG_ID|g" fsx-storage-class.yaml

# Cập nhật PV với chi tiết FSx Lustre
sed -i "s|<fs-id>|$FSX_ID|g" fsx-lustre-pv.yaml
sed -i "s|<fs-id>.fsx.us-west-2.amazonaws.com|$FSX_DNS|g" fsx-lustre-pv.yaml
sed -i "s|<mount-name>|$FSX_MOUNT|g" fsx-lustre-pv.yaml

# Áp dụng tài nguyên Kubernetes
kubectl apply -f fsx-storage-class.yaml
kubectl apply -f fsx-lustre-pv.yaml
kubectl apply -f fsx-lustre-pvc.yaml
```

## Cài đặt AWS Load Balancer Controller

Để expose dịch vụ vLLM ra bên ngoài, chúng ta cài đặt AWS Load Balancer Controller:

```bash
# Tải tài liệu IAM policy
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/main/docs/install/iam_policy.json

# Tạo IAM policy
aws --profile vllm-profile iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam-policy.json

# Tạo IAM OIDC provider cho cụm
eksctl utils associate-iam-oidc-provider --profile vllm-profile --region=us-west-2 --cluster=vllm-cluster --approve

# Tạo IAM service account cho AWS Load Balancer Controller
ACCOUNT_ID=$(aws --profile vllm-profile sts get-caller-identity --query "Account" --output text)
eksctl create iamserviceaccount \
  --profile vllm-profile \
  --cluster=vllm-cluster \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --attach-policy-arn=arn:aws:iam::$ACCOUNT_ID:policy/AWSLoadBalancerControllerIAMPolicy \
  --override-existing-serviceaccounts \
  --approve

# Cài đặt AWS Load Balancer Controller sử dụng Helm
helm repo add eks https://aws.github.io/eks-charts
helm repo update

# Cài đặt CRDs
kubectl apply -f https://raw.githubusercontent.com/aws/eks-charts/master/stable/aws-load-balancer-controller/crds/crds.yaml

# Cài đặt controller
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=vllm-cluster \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller

# Cài đặt LeaderWorkerSet controller
helm install lws oci://registry.k8s.io/lws/charts/lws \
  --version=0.6.1 \
  --namespace lws-system \
  --create-namespace \
  --wait --timeout 300s
```

## Cấu hình security groups cho ALB

Chúng ta tạo một security group chuyên dụng cho ALB và cấu hình nó để cho phép lưu lượng inbound trên port 80:

```bash
# Tạo security group cho ALB
USER_IP=$(curl -s https://checkip.amazonaws.com)

ALB_SG=$(aws --profile vllm-profile ec2 create-security-group \
  --group-name vllm-alb-sg \
  --description "Security group for vLLM ALB" \
  --vpc-id $VPC_ID \
  --query "GroupId" --output text)

echo "ALB security group: $ALB_SG"

# Cho phép lưu lượng inbound trên port 80 từ IP của bạn
aws --profile vllm-profile ec2 authorize-security-group-ingress \
  --group-id $ALB_SG \
  --protocol tcp \
  --port 80 \
  --cidr ${USER_IP}/32

# Lấy node group security group ID
NODE_INSTANCE_ID=$(aws --profile vllm-profile ec2 describe-instances \
  --filters "Name=tag:eks:nodegroup-name,Values=vllm-p4d-nodes-efa" \
  --query "Reservations[0].Instances[0].InstanceId" --output text)

NODE_SG=$(aws --profile vllm-profile ec2 describe-instances \
  --instance-ids $NODE_INSTANCE_ID \
  --query "Reservations[0].Instances[0].SecurityGroups[0].GroupId" --output text)

# Cho phép lưu lượng từ ALB security group đến node security group trên port 8000
aws --profile vllm-profile ec2 authorize-security-group-ingress \
  --group-id $NODE_SG \
  --protocol tcp \
  --port 8000 \
  --source-group $ALB_SG

# Cập nhật security group trong file ingress
sed -i "s|<sg-id>|$ALB_SG|g" vllm-deepseek-32b-lws-ingress.yaml
```

## Triển khai máy chủ vLLM

Cuối cùng, chúng ta triển khai máy chủ vLLM sử dụng mẫu LeaderWorkerSet:

```bash
# Triển khai máy chủ vLLM
kubectl apply -f vllm-deepseek-32b-lws.yaml

# Giám sát trạng thái pod
kubectl get pods

# Kiểm tra logs của pod
kubectl logs -f <pod-name>
```

Việc triển khai sẽ bắt đầu ngay lập tức, nhưng pod có thể vẫn ở trạng thái ContainerCreating trong vài phút (5–15 phút) trong khi pull container image lớn hỗ trợ GPU.

```bash
# Áp dụng ingress
kubectl apply -f vllm-deepseek-32b-lws-ingress.yaml

# Kiểm tra trạng thái ingress
kubectl get ingress
```

## Kiểm tra triển khai

Khi triển khai hoàn tất, chúng ta có thể kiểm tra máy chủ vLLM:

```bash
# Kiểm tra máy chủ vLLM
export VLLM_ENDPOINT=$(kubectl get ingress vllm-deepseek-32b-lws-ingress -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
echo "vLLM endpoint: $VLLM_ENDPOINT"

# Kiểm tra completions API
curl -X POST http://$VLLM_ENDPOINT/v1/completions \
  -H "Content-Type: application/json" \
  -d '{
      "model": "deepseek-ai/DeepSeek-R1-Distill-Qwen-32B",
      "prompt": "Hello, how are you?",
      "max_tokens": 100,
      "temperature": 0.7
  }'
```

## Các cân nhắc về hiệu suất

### Elastic Fabric Adapter

EFA cung cấp những lợi ích hiệu suất đáng kể cho các khối lượng công việc suy luận phân tán:

- **Giảm độ trễ** – Độ trễ thấp hơn và nhất quán hơn cho giao tiếp giữa các GPU trên các nút
- **Thông lượng cao hơn** – Thông lượng cao hơn cho việc truyền dữ liệu giữa các nút
- **Cải thiện khả năng mở rộng** – Hiệu quả mở rộng tốt hơn trên nhiều nút
- **Hiệu suất tốt hơn** – Cải thiện đáng kể hiệu suất cho các khối lượng công việc suy luận phân tán

### Tích hợp FSx for Lustre

Sử dụng FSx for Lustre để lưu trữ mô hình cung cấp nhiều lợi ích:

- **Lưu trữ bền vững** – Trọng số mô hình được lưu trữ trên hệ thống file FSx for Lustre và tồn tại qua các lần khởi động lại pod
- **Tải nhanh hơn** – Sau lần tải xuống đầu tiên, việc tải mô hình nhanh hơn nhiều
- **Lưu trữ chia sẻ** – Nhiều pod có thể truy cập cùng các trọng số mô hình
- **Hiệu suất cao** – FSx for Lustre cung cấp truy cập thông lượng cao, độ trễ thấp đến trọng số mô hình

## Dọn dẹp

Để tránh phát sinh thêm phí, hãy dọn dẹp các tài nguyên được tạo trong bài viết này:

```bash
chmod +x cleanup.sh
./cleanup.sh
```

## Kết luận

Trong bài viết này, chúng tôi đã trình bày cách triển khai mô hình DeepSeek-R1-Distill-Qwen-32B trên Amazon EKS sử dụng vLLMs, với hỗ trợ GPU, EFA, và tích hợp FSx for Lustre. Kiến trúc này cung cấp một hệ thống có thể mở rộng, hiệu suất cao để phục vụ các khối lượng công việc suy luận LLM.

AWS Deep Learning Containers cho vLLM cung cấp một môi trường được đơn giản hóa, tối ưu hóa để đơn giản hóa việc triển khai LLM bằng cách giảm thiểu sự phức tạp của cấu hình môi trường, quản lý phụ thuộc, và điều chỉnh hiệu suất. Bằng cách sử dụng những container được cấu hình trước này, các tổ chức có thể giảm thời gian triển khai và tập trung vào việc tạo ra giá trị từ các ứng dụng LLM của họ.

Bằng cách kết hợp AWS DLCs với Amazon EKS, instances P4d với NVIDIA A100 GPUs, EFA, và FSx for Lustre, bạn có thể đạt được hiệu suất tối ưu cho suy luận LLM trong khi vẫn duy trì tính linh hoạt và khả năng mở rộng của Kubernetes.

Giải pháp này giúp các tổ chức:

- Triển khai LLMs hiệu quả ở quy mô lớn
- Tối ưu hóa việc sử dụng tài nguyên GPU với container orchestration
- Cải thiện hiệu suất mạng giữa các nút với EFA
- Tăng tốc việc tải mô hình với lưu trữ hiệu suất cao
- Cung cấp API suy luận có thể mở rộng, hiệu suất cao

Code và file cấu hình hoàn chỉnh cho việc triển khai này có sẵn trong [repository GitHub](https://github.com/aws-samples/sample-aws-deep-learning-containers) của chúng tôi. Chúng tôi khuyến khích bạn thử nghiệm và điều chỉnh nó cho trường hợp sử dụng cụ thể của bạn.

---

## Về các tác giả

### Vishal Naik
![Vsnak.jpg](/images/3-blog/3-image/Vsnak.jpg)
Vishal Naik là Solutions Architect Cấp cao tại Amazon Web Services (AWS). Anh ấy là một builder thích giúp khách hàng đạt được nhu cầu kinh doanh và giải quyết các thách thức phức tạp với các giải pháp AWS và thực hành tốt nhất. Lĩnh vực tập trung chính của anh ấy bao gồm Generative AI và Machine Learning. Trong thời gian rảnh rỗi, Vishal thích làm phim ngắn về chủ đề du hành thời gian và vũ trụ thay thế.

### Sumeet Tripathi
![Sumeettr.jpg](/images/3-blog/3-image/Sumeettr.jpg)
Sumeet Tripathi là Enterprise Support Lead (TAM) tại AWS ở North Carolina. Anh ấy có hơn 17 năm kinh nghiệm trong công nghệ qua nhiều vai trò khác nhau. Anh ấy đam mê giúp khách hàng giảm thiểu các thách thức và ma sát vận hành. Lĩnh vực tập trung của anh ấy là AI/ML và Phân khúc Năng lượng & Tiện ích. Ngoài công việc, anh ấy thích đi du lịch với gia đình, xem cricket và phim.
```