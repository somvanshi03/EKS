###########    EKS Storage  ############

# EKS Storage with EBS 
1. Create IAM policy for EBS
2. attach IAM policy to worker node
3. install EBS CSI driver

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:CreateVolume",
        "ec2:DeleteVolume",
        "ec2:AttachVolume",
        "ec2:DetachVolume",
        "ec2:DescribeVolumes",
        "ec2:ModifyVolume",
        "ec2:CreateSnapshot",
        "ec2:DeleteSnapshot",
        "ec2:DescribeSnapshots",
        "ec2:CopySnapshot",
        "ec2:DescribeVolumeStatus",
        "ec2:EnableVolumeIO",
        "ec2:CreateVolumePermission",
        "ec2:ModifySnapshotAttribute",
        "ec2:DescribeVolumeSnapshots"
      ],
      "Resource": "*"
    }
  ]
}



1. EBS Storage:
---------------
# StorageClass

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: ebs-sc
provisioner: ebs.csi.aws.com
parameters:
  type: gp2
reclaimPolicy: Retain
volumeBindingMode: WaitForFirstConsumer


---------------------------
# PV
apiVersion: v1
kind: PersistentVolume
metadata:
  name: ebs-pv
spec:
  capacity:
    storage: 10Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: ebs-sc
  awsElasticBlockStore:
    volumeID: <your-volume-id>
    fsType: ext4


-----------------------
# PVC 
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ebs-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: ebs-sc


----------------------------
# Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ebs-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: ebs-app
  template:
    metadata:
      labels:
        app: ebs-app
    spec:
      containers:
        - name: nginx
          image: nginx
          volumeMounts:
            - mountPath: /usr/share/nginx/html
              name: ebs-storage
      volumes:
        - name: ebs-storage
          persistentVolumeClaim:
            claimName: ebs-pvc
