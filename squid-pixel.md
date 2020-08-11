# Squid Pixel

## Sobre

> Um script de integração da Squid que faz todo tracking de cadastros e vendas.

## Comportamento

O componente squid-pixel se comporta como um pixel de rastreamento simples. Ele usa uma única URL, mas fornece variáveis que podem ser substituídas pelo componente na string de URL ao fazer a solicitação.

### Link de influenciador Squid

Para que o SquidPixel consiga receber parâmetros de um influenciador Squid. O link da landing page precisa ter uma querystring no seguinte formato: `?squid-share=CODIGO`. Este código é armazenado no dispositivo de acesso e com ele validamos se a conversão de venda ou cadastro foi gerada através do link do influenciador.

### Cupom de influenciador Squid

O SquidPixel também monitora conversões (normalmente vendas) feitas com cupom de influenciador Squid. O código desse cupom deve ser mandado junto ao registro de sucesso da conversão (usando variáveis da landing page). Veja: `cupom_used` na seção `2 - Script de registro de conversão`

### Valor em vendas

Para registrar valor em vendas pelo SquidPixel é necessário enviar junto ao registro de conversão o valor do sucesso do carrinho (usando variáveis da landing page). Veja: `order_value` na seção `2 - Script de registro de conversão`

## Integração - Lista de scripts da página ou via GTM

### 1 - Script de inicialização do SquidPixel:

Esse script serve para iniciar o SquidPixel na landing page e para validar se o link de origem é de influenciador Squid.

#### Script:

```
<script>
(function(d,s,id){  chk=function(cb){if(window.squidPixelLoaded){cb();}else{window.setTimeout(function(){chk(cb);},100);}};
  b={};b.init=function(a){chk(function(){TP.init(a);});};
  b.registerSell=function(so){chk(function(){TP.registerSell(so);});};
  b.registerProduct=function(pi){chk(function(){TP.registerProduct(pi);});};
  SquidPixel=b;if(!window.squidPixelLoaded){var js,te=d.getElementsByTagName(s)[0];if(d.getElementById(id)){return;}
  js=d.createElement(s);js.id=id;
  js.src="https://cdn2.squidit.com.br/referral/squid-pixel.js";te.parentNode.insertBefore(js,te);}
  }(document,'script','squid-pixel'));
  SquidPixel.init("[[ID DA ORGANIZAÇAO DO CLIENTE]]");
</script>
```

##### Atributos:

`[[ID DA ORGANIZAÇAO DO CLIENTE]`: Id da organização na Squid usado para validar o link ou cupom do influenciador

##### Instalação:

Esse script precisa ser instalado de modo a ser inicializado junto com a landing page. Exemplo no rodapé da página. E precisa ser acessado de todas as páginas da landing page, principalmente na tela inicial e na tela de sucesso de cadastro ou carrinho.

##### Testando instalação:

Para se certificar que essa etapa da instalação foi concluída com sucesso siga o passo-a-passo:

- Abra o console do navegador

- Coloque a query string `?squid-share=testpixel` na url da landing page.

- Verifique se tem esses dois registros:

`Squid - Pixel initiated - organization: ID DA ORGANIZAÇAO DO CLIENTE`

`Squid - Share code validated`

*A nossa API vai validar que é um link de teste e vai validar como um link de influenciador Squid*

### 2 - Script de registro de conversão:

Esse script serve para registrar conversões na base da Squid.

#### Script:

```
<script>
  SquidPixel.registerSell({
    order_id: [[__]],
    order_value: [[__]], //linha opcional
    cupom_used: [[__]], //linha opcional
    other_info: {
      [[__]] //linha opcional
    }
  })
</script>
```

##### Atributo Obrigatório

`order_id` ID ÚNICO  DA CONVERSÃO DO ECOMMERCE, POR EXEMPLO `order_id: "ORDERID12345"` - esse Id é importante para fins de audiotoria das conversões junto a plataforma que está integrando o squid-pixel.

##### Atributos Opcionais

`order_value` VALOR DO CARRINHO SE FOR GAMIFICAR VALOR EM VENDAS, POR EXEMPLO `order_value: 100`

`cupom_used` CÓDIGO DO CUPOM SE FOR USADO NA COMPRA, POR EXEMPLO `cupom_used: "SQUID10"`

`other_info` QUALQUER INFORMAÇÃO PERTINENTE A GUARDAR NA CONVERSÃO, POR EXEMPLO `other_info: {emaildecadastro: "email@exemplo.com"}`

#### Instalação:

Este script precisa ser instalado na tela de sucesso de cadastro ou carrinho. Pode estar no html da página ou pode ser chamado via JS no evento de sucesso de cadastro ou carrinho.

##### Testando instalação:

Para se certificar que essa etapa da instalação foi concluída com sucesso siga o passo a passo:

- Abra o console do navegador;

- Coloque a query string `?squid-share=testpixel` na url da landing page;

- Gere uma conversão em sua landing page;

- Verifique se tem esses três registros:

`Squid - Pre-convertion registered.`

`Squid - Share code exists`

`Squid - Sell registered`

*A nossa API vai validar que é um link de teste e vai registrar a conversão de teste como se fosse de um influenciador Squid*
