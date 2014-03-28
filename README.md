Esri Apps e Autenticação OAuth
==================

Esta aplicação demonstra os principais conceitos relacionados aos cenários de autenticação de aplicações no [ArcGIS Online](http://www.arcgis.com). 

A utilização de serviços tarifados na plataforma ArcGIS Online demanda a associação de um usuário do serviço. Ainda assim, é possível utilizar uma estratégia de _proxying_ para permitir que usuários finais de sua app possam utilizar serviços Esri, sem necessidade de autenticação. Neste cenário, a autenticação é realizada por um token atribuído à aplicação e todo o consumo de créditos é debitado da conta associada à aplicação.

### Configurando uma aplicação no [ArcGIS for Developers](http://developers.arcgis.com)

Para realizar a autenticação através de sua aplicação, você vai precisar obter um identificador da aplicação e um token de acesso. Para isso, siga os seguintes passos:

1. Acesse (ou crie) sua conta de desenvolvedor na plataforma Esri no endereço [http://developers.arcgis.com](http://developers.arcgis.com);
2. Acesse o menu _Applications_ e clique no botão _New Application_;
3. Cadastre a nova aplicação preenchendo os campos solicitados;
4. Clique sobre a opção _OAuth Credentials_. Os campos necessários para autenticação pela sua app são apresentados nesta interface. __Atenção:__ Jamais divulgue estas informações, elas serão utilizadas para tarifar a sua conta no ArcGIS Online pelo uso dos serviços Esri. Veja mais detalhes na seção __Mantendo o clientId e o secretId seguros__.

### Autenticação de sua app e uso da proxy page

Uma das [possíveis formas](https://developers.arcgis.com/authentication/app-logins.html) de autenticação da sua app é através da utilzação de uma [proxy page](https://github.com/Esri/resource-proxy). Neste repositório, você encontra a configuração da proxy page para esta aplicação no diretório PHP (você vai precisar de um runtime PHP para executá-la), arquivo proxy.config. O arquivo proxy.config possui a seguinte estrutura:

    <serverUrls>
        <serverUrl url="http://services.arcgisonline.com"
                   matchAll="true"
                   clientId="YOUR_CLIENT_ID"
                   oauth2Endpoint="https://www.arcgis.com/sharing/oauth2/token"
                   clientSecret="YOUR_CLIENT_SECRET"/>

        <serverUrl url="http://route.arcgis.com"
                   matchAll="true"
                   clientId="YOUR_CLIENT_ID"
                   oauth2Endpoint="https://www.arcgis.com/sharing/oauth2/token"
                   clientSecret="YOUR_CLIENT_SECRET"/>


        <serverUrl url="http://traffic.arcgis.com"
                   matchAll="true"
                   clientId="YOUR_CLIENT_ID"
                   oauth2Endpoint="https://www.arcgis.com/sharing/oauth2/token"
                   clientSecret="YOUR_CLIENT_SECRET"/>
    </serverUrls>


Execute a aplicação. As informações atuais de clientID e clientSecret não são conhecidas pelo ArcGIS Online e, portanto, você vai receber a janela de autenticação solicitando a inserção de um usuário do ArcGIS Online. Como este não é o comportamento desejado, altere os valores das chaves clientId e clientSecret para os obtidos anteriormente (no passo 4). Feche o browser e abra novamente a aplicação. Você vai observar que a janela de autenticação desapareceu e é possível realizar a operação de rota sem a necessidade de utilização de uma conta do ArcGIS Online pelo usuário final (a partir de agora, a conta tarifada pela utilização dos serviços é a conta associada à aplicação e não mais a conta do usuário final).

### Mantendo o clientId e o secretId seguros

É sua responsabilidade manter a confidencialidade das informações de clientId e secretId. Entenda que qualquer pessoa que estiver de posse destas informações pode autenticar-se à plataforma ArcGIS e utilizar estas credencias para utilizar os serviços Esri (que serão cobrados da sua conta). Desta forma, jamais envie as informações de clientId e secredId para o cliente. Uma boa alternativa para lidar com essa questão de segurança destas informações é utilizar o conceito de proxy page previamente proposto.
