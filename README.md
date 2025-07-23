## 🔐 IAM (Identity and Access Management)

### ✅ Criar usuário IAM
```bash
aws iam create-user --user-name <nome-do-usuario>
```

### ✅ Criar role com política inline
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::nome-do-bucket/*"
    }
  ]
}
```

### ✅ Anexar política a uma role
```bash
aws iam put-role-policy   --role-name minha-role   --policy-name politica-s3   --policy-document file://policy.json
```

---

## 🖥️ EC2

### ✅ Conectar via SSH
```bash
ssh -i minha-chave.pem ec2-user@<IP-da-instancia>
```

### ✅ Instalar Apache e iniciar
```bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

---

## ☁️ S3

### ✅ Criar bucket
```bash
aws s3 mb s3://meu-bucket-jam
```

### ✅ Enviar arquivos
```bash
aws s3 cp index.html s3://meu-bucket-jam/
```

### ✅ Tornar objetos públicos (ACL)
```bash
aws s3api put-object-acl --bucket meu-bucket-jam --key index.html --acl public-read
```

---

## 🛢️ RDS / Redshift

### ✅ Criar usuário no Redshift
```sql
CREATE USER meuusuario PASSWORD 'MinhaSenhaForte123';
```

### ✅ Conceder acesso por colunas
```sql
GRANT SELECT (s_name, s_segment, s_dietrestrictions) ON TABLE sailors TO crew;
```

### ✅ Copiar dados do S3 para Redshift
```sql
COPY sailors
FROM 's3://bucket/path'
IAM_ROLE 'arn:aws:iam::123456789012:role/RedshiftRole'
FORMAT AS PARQUET;
```

---

## 📦 ECS com EC2

### ✅ Registrar instância no cluster ECS
```bash
sudo vim /etc/ecs/ecs.config
ECS_CLUSTER=nome-do-cluster
sudo systemctl restart ecs
```

---

## 🧬 Lambda

### ✅ Adicionar permissão ao KMS para uso na Lambda
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowLambdaUse",
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "kms:*",
      "Resource": "*"
    }
  ]
}
```

---

## ⚡ DynamoDB

### ✅ Criar tabela com chave composta
```bash
aws dynamodb create-table   --table-name character_data   --attribute-definitions AttributeName=id,AttributeType=S AttributeName=username,AttributeType=S   --key-schema AttributeName=id,KeyType=HASH AttributeName=username,KeyType=RANGE   --billing-mode PROVISIONED   --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5
```

---

## 🧪 Dicas Finais

- 📍 Sempre verifique a **região** (`us-east-1`, `us-west-2`, etc)
- 📍 Leia com atenção o enunciado da task – ele sempre traz *nomes específicos* de recursos!
- ✅ Use `AWS CLI` sempre que possível – é mais rápido e direto.
- 🚨 Alguns erros comuns:
  - `PermanentRedirect`: o bucket S3 está em outra região
  - `invalid CREDENTIALS clause`: faltando o `IAM_ROLE` no `COPY`
  - `AccessDenied`: falta permissão (verifique política ou role da Lambda)

---

## 📂 Como usar no Git

```bash
git init
git add README.md
git commit -m "Guia rápido para AWS Jam"
git branch -M main
git remote add origin https://github.com/<seu-usuario>/aws-jam-notes.git
git push -u origin main
```

Boa prova! 🚀
