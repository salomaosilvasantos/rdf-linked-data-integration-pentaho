## Tryplify Process##

 ***TryplifyProcess*** é uma transformação realizada com o intuito de mapear e converter dados do [**Tribunal de Contas da União** (**TCU**)](http://portal2.tcu.gov.br/portal/page/portal/TCU/contas_publicas/inicio) para o para o formato final **Resource Description Framework** (**RDF**).  

# **Instruções para utilização da transformação** #


### 1.  Importar e execultar o projeto ###

   - Com o ***kettle*** já instalado, basta clonar ou baixar do gitHub o [Projeto Linked-Data-Integration](https://github.com/ARiDa/linked-data-integration).      
     
   - Descompactar o arquivo **linked-data-integration-master.zip** em uma pasta de sua preferência.
   
   - Para **importar** o projeto para a ferramenta Spoon, selecione a opção **File > Open** e escolha o arquivo com extensão **.ktr**
   
   - Para **executar a Transformation** selecione a opção **Action > Run** ou **F9**.  Aparecerá uma tela de configuração, basta selecionar a opção **Launch**.
  

# **Detalhes da transformação *TryplifyProcess*** #
 - **CSV file input** > nesse step é realizada a leitura do do arquivo original no formato CSV.
 
 - **Select values** > nesse step é nomeado cada coluna lida no arquivo CSV. 
	     **Exemplo:**	Field_000 > name

 - **Replace in string** > realiza a substituição de (**\ . -**) por espaço em         branco no campo CPF\CNPJ, usando expressão regular.
        Resumindo, trato CPF\CNPJ.	Expressão Regula utilizada retira todos os campos de uma String que não seja números. **ER: ([^\d])**

 - **Filter rows CPF** > realiza o filtro de CPF, ou seja, é utilizado expressão 
   regular para escolher números com 11 dígitos. **ER: ^\d{11}$**. Se a condição for falsa ele passa para o step **Filter rows CNPJ**.
 
 -  **Filter rows CNPJ** > realiza o filtro de CPNPJ, ou seja, é utilizado expressão regular para escolher números com 14 dígitos. **ER: ^\d{14}$**. Se a condição for falsa ele passa para o step **Erros**, escrevendo os números que tem a quantidade de dígitos diferente de 09 ou 11, em um arquivo.
 
 - **Base URI** > Nesse Step é criado uma constante, a constante é a URI Base.
    O nome definido para a constante é **Base URI** do tipo String que recebeu o valor **http://arida.ufc.br**.

 -  **URI Resource** > Nesse Step é criado uma constante, a constante é a URI do recurso.  O nome definido para a constante é **Recurso** do tipo String que recebeu o valor **Empresa**.     
         
 - **Formula** > Nesse Step é criado uma fórmula para resultar na URI. O nome definido é **URI** e o valor da formula é **[Base URI]&"/resource/"&[Recurso]&"/"& int([cpf/cnpj])**
 
 - **Data Property Mapping** > Nesse Step é definido a ontologia e é realizado a associação entre o Predicado(Data Property) e seu respectivo campo do Objeto.    
  - A **ontologia** definida recebe o valor **<http://arida.ufc.br/ontology/Empresa>** .                       
  - Já na associação ocorre o seguinte: o valor do **Predicado** é **<http://www.arida.ufc.br/ontology/name>** é igual ao campo **name** .                                                               
                                         

 - **Scape** > Nesse Step é criado uma constante, a constante é a aspas duplas.  O nome definido para a constante é **Scape** do tipo String que recebeu o valor aspas duplas  **(")**.
 
 - **Formula 3** > Nesse Step é criado uma fórmula para resultar na tripla. O nome definido é **tripla** e o valor da formula é **"<" & T([subject]) & "> " & if (left(T([predicate]); 1) <>"<"; "<" & T([predicate]) & "> "; T([predicate])) &" "& if (left(T([predicate]); 1) ="<"; T([scape]) & T([object]) & T([scape]); T([object])) & " ."** .
 
 - **Triplas Data Property** > nesse step é realizado a seleção apenas do campo **tripla**.
 
 - **Saida RDF** > nesse step é definido o caminho de saída com o nome do arquivo e sua extensão. O caminho com nome é **${Internal.Transformation.Filename.Directory}\saida** e a extensão recebe o valor **nt**.








 



