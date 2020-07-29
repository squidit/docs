# Squid Pixel

## Sobre

> Um script de integração da Squid que faz todo tracking de cadastros, vendas, acessos e cliques.

## Comportamento

O componente squid-pixel se comporta como um pixel de rastreamento simples. Ele usa uma única URL, mas fornece variáveis que podem ser substituídas pelo componente na string de URL ao fazer a solicitação.

## Exemplo integração

### Lista de scripts da página ou via GTM

`
<script>
(function(d,s,id){  chk=function(cb){if(window.squidPixelLoaded){cb();}else{window.setTimeout(function(){chk(cb);},100);}};
  b={};b.init=function(a){chk(function(){TP.init(a);});};
  b.registerSell=function(so){chk(function(){TP.registerSell(so);});};
  b.registerProduct=function(pi){chk(function(){TP.registerProduct(pi);});};
  SquidPixel=b;if(!window.squidPixelLoaded){var js,te=d.getElementsByTagName(s)[0];if(d.getElementById(id)){return;}
  js=d.createElement(s);js.id=id;
  js.src="https://cdn2.squidit.com.br/referral/squid-pixel.js";te.parentNode.insertBefore(js,te);}
  }(document,'script','squid-pixel'));
  SquidPixel.init("[[ID DA ORGANIZAÇ O DO CLIENTE]]");
</script>
`

E neste exemplo mostramos o squid-pixel na tela de conversão 

`
<script>
  SquidPixel.registerSell({
    order_id: [[__]],
    order_value: [[__]],
    cupom_used: [[__]],
    other_info: {
      [[__]]
    }
  })
</script>
`

### Atributos

`order_id` ID ÚNICO  DA CONVERSÃO DO ECOMMERCE
`order_value` VALOR DO CARRINHO SE FOR GAMIFICAR VALOR EM VENDAS
`cupom_used` CÓDIGO DO CUPOM SE FOR USADO NA COMPRA]
`other_info` QUALQUER INFORMAÇÃO PERTINENTE A GUARDAR NA CONVERSÃO
