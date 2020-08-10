# Squid Pixel

## Sobre

> Um script de integração da Squid que faz todo tracking de cadastros, vendas, acessos e cliques.

## Comportamento

O componente squid-pixel se comporta como um pixel de rastreamento simples. Ele usa uma única URL, mas fornece variáveis que podem ser substituídas pelo componente na string de URL ao fazer a solicitação.

## Exemplo integração

### Lista de scripts da página ou via GTM

`<script>
(function(d,s,id){  chk=function(cb){if(window.squidPixelLoaded){cb();}else{window.setTimeout(function(){chk(cb);},100);}};
  b={};b.init=function(a){chk(function(){TP.init(a);});};
  b.registerSell=function(so){chk(function(){TP.registerSell(so);});};
  b.registerProduct=function(pi){chk(function(){TP.registerProduct(pi);});};
  SquidPixel=b;if(!window.squidPixelLoaded){var js,te=d.getElementsByTagName(s)[0];if(d.getElementById(id)){return;}
  js=d.createElement(s);js.id=id;
  js.src="https://cdn2.squidit.com.br/referral/squid-pixel.js";te.parentNode.insertBefore(js,te);}
  }(document,'script','squid-pixel'));
  SquidPixel.init("[[ID DA ORGANIZAÇ O DO CLIENTE]]");
</script>`

E neste exemplo mostramos o squid-pixel na tela de conversão 

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

### Atributo Obrigatório

`order_id` ID ÚNICO  DA CONVERSÃO DO ECOMMERCE, POR EXEMPLO `order_id: "ORDERID12345"` - esse Id é importante para fins de audiotoria das conversões junto a plataforma que está integrando o squid-pixel.

### Atributos Opcionais

`order_value` VALOR DO CARRINHO SE FOR GAMIFICAR VALOR EM VENDAS, POR EXEMPLO `order_value: 100`

`cupom_used` CÓDIGO DO CUPOM SE FOR USADO NA COMPRA, POR EXEMPLO `cupom_used: "SQUID10"`

`other_info` QUALQUER INFORMAÇÃO PERTINENTE A GUARDAR NA CONVERSÃO `other_info: {emaildecadastro: "email@exemplo.com"}`
