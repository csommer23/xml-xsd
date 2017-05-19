## Exemplo 1 ##
```xml
<!--Esse é no mesmo arquivo extensão .xsd-->
<simpleType name="CPF"> <!--Aqui como se fosse um method com uma regra regex para CPF-->
    <restriction base="string">
        <pattern value="\d{3}\.\d{3}\.\d{3}-\d{2}" />
    </restriction>
</simpleType>

<complexType name="Pessoa" abstract="true"> <!--Aqui é como se fosse uma class Pessoa-->
    <sequence>
        <element name="nome" type="string" />
    </sequence>
</complexType>

<complexType name="PessoaFisica">
    <complexContent>
        <extension base="tns:Pessoa"> <!--extend class Pessoa (tns faz isso)-->
            <sequence>
                <element name="cpf" type="tns:CPF" /> <!--call method CPF (tns faz isso)-->
            </sequence>
        </extension>
    </complexContent>
</complexType>

<!--Como fica no arquivo .xml-->
<pessoaFisica>
    <nome>Alexandre</nome>
    <cpf>123.456.789-09</cpf>
</pessoaFisica>
```
## Exemplo 2 ##
```xml
<!--Um exemplo de criação de um atributo em um elemento-->
<complexType name="Pessoa" abstract="true">
    <attribute name="id" type="long" /> 
</complexType>

<!--Como fica no arquivo .xml-->
<Pessoa id="1"/>
```
## Exemplo 3 ##
```xml
<!--Aqui tem um exemplo de dois arquivos primeiro de endereço, e o outro pessoa, e pessoa está chamando o arquivo endereço;-->

<!--Arquivo Endereco.xsd-->
<?xml version="1.0" encoding="UTF-8"?>
<schema targetNamespace="http://geladaonline.com.br/endereco/v1"
        elementFormDefault="qualified"
        xmlns="http://www.w3.org/2001/XMLSchema"
        xmlns:tns="http://geladaonline.com.br/endereco/v1">

    <simpleType name="CEP">
        <restriction base="string">
            <pattern value="\d{5}-\d{3}" />
        </restriction>
    </simpleType>

    <complexType name="Endereco">
        <sequence>
            <element name="cep" type="tns:CEP" />
            <element name="logradouro" type="string" />
        </sequence>
    </complexType>

</schema>

<!--Arquivo Pessoa.xsd-->
<?xml version="1.0" encoding="UTF-8"?>
<schema targetNamespace="http://geladaonline.com.br/pessoa/v1"
        xmlns="http://www.w3.org/2001/XMLSchema"
        xmlns:tns="http://geladaonline.com.br/pessoa/v1"
        xmlns:end="http://geladaonline.com.br/endereco/v1">

    <import namespace="http://geladaonline.com.br/endereco/v1" schemaLocation="Endereco.xsd" /> <!--importando o arquivo de endereço (obs: preciso da ref xmlns:end)-->

    <complexType name="Pessoa">
        <sequence>
            <element name="endereco" type="end:Endereco" minOccurs="0" minOccurs="2"/> <!--dei um call do arquivo e estou dizendo que não é obrigatorio e no máximo 2-->
        </sequence>
    </complexType>

</schema>

<!--Como fica no arquivo .xml-->
<Pessoa>
    <endereco>
        <cep>12345-678</cep>
        <logradouro>Rua Um</logradouro>
    </endereco>
    <endereco>
        <cep>87654-321</cep>
        <logradouro>Rua Dois</logradouro>
    </endereco>
</Pessoa>
```
