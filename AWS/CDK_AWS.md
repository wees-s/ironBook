**AWS CDK**
___
Nele você pode **definir toda sua infraestrutura utilizando linguagens de programação como (Python, typescript, etc.)**. Ele provisiona tudo automaticamente na AWS.
Dessa forma você pode configurar todo seu ambiente sem a necessidade de ir na interface manualmente.

> _Como funciona?_   
> _R: Ele gera em seu background templates AWS CloudFormation que executa a criação dos recursos._

A linguagem mais comum usada é typescript.   
Exemplo 1 de uso:   
```typescript
import * as cdk from 'aws-cdk-lib';
import { Construct } from 'constructs';
import * as s3 from 'aws-cdk-lib/aws-s3';

export class MyFirstStack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    new s3.Bucket(this, 'MyBucket', {
      bucketName: 'meu-bucket-exemplo-123',
      versioned: true,
      removalPolicy: cdk.RemovalPolicy.DESTROY, // cuidado em produção
    });
  }
}
```
___

> ```cdk init app --language typescript```  
> ```• coloca esse código no stack```  
> ```cdk deploy```  

Exemplo 2 de uso:
```typescript
import * as ec2 from 'aws-cdk-lib/aws-ec2';

const vpc = new ec2.Vpc(this, 'MyVPC');

new ec2.Instance(this, 'MyInstance', {
  vpc,
  instanceType: new ec2.InstanceType('t2.micro'),
  machineImage: ec2.MachineImage.latestAmazonLinux(),
});
```
___

> ```• cria uma VPC (rede)```  
> ```• cria uma instância EC2 (sua VM)```  
> ```• configura imagem e tipo - automático no cmd: cdk deploy```   

O CDK vai gerar um template do AWS CloudFormation e provisionar o recurso.
