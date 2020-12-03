# Recursos de apoio para geração de QR Codes estáticos do Pix em PHP - WDEV

Repositório complementar do canal [WDEV](https://youtube.com/wdevoficial) para a geração do payload do QR code Pix com PHP.

Nesse repositório você vai encontrar constantes com IDs dos objetos do payload do Pix e método de cálculo da verificação cíclica de redundância (CRC16).

___________________

## Vídeo WDEV

Para assistir o vídeo dessa implementação, acesse: [Integração Pix PHP: Gerando payload e QR Code estático do Pix com PHP (YouTube)](https://youtu.be/eO11iFgrdCA)
___________________

## Documentação base do BACEN

[Manual de padrões para iniciação do Pix](https://www.bcb.gov.br/content/estabilidadefinanceira/pix/Regulamento_Pix/II-ManualdePadroesparaIniciacaodoPix-versao2-1.pdf)
___________________

## Constantes com IDs do payload

Para facilitar a implementação do payload Pix, seguem abaixo as constantes que usaremos no projeto:
```php
  /**
   * IDs do Payload do Pix
   * @var string
   */
  const ID_PAYLOAD_FORMAT_INDICATOR = '00';
  const ID_MERCHANT_ACCOUNT_INFORMATION = '26';
  const ID_MERCHANT_ACCOUNT_INFORMATION_GUI = '00';
  const ID_MERCHANT_ACCOUNT_INFORMATION_KEY = '01';
  const ID_MERCHANT_ACCOUNT_INFORMATION_DESCRIPTION = '02';
  const ID_MERCHANT_CATEGORY_CODE = '52';
  const ID_TRANSACTION_CURRENCY = '53';
  const ID_TRANSACTION_AMOUNT = '54';
  const ID_COUNTRY_CODE = '58';
  const ID_MERCHANT_NAME = '59';
  const ID_MERCHANT_CITY = '60';
  const ID_ADDITIONAL_DATA_FIELD_TEMPLATE = '62';
  const ID_ADDITIONAL_DATA_FIELD_TEMPLATE_TXID = '05';
  const ID_CRC16 = '63';
```

___________________

## Verificação cíclica de redundância (CRC16)

Para facilitar a implementação do payload Pix, segue abaixo o método de cálculo da hash de verificação CRC16 exigida:
```php
  /**
   * Método responsável por calcular o valor da hash de validação do código pix
   * @return string
   */
  private function getCRC16($payload) {
      //ADICIONA DADOS GERAIS NO PAYLOAD
      $payload .= self::ID_CRC16.'04';

      //DADOS DEFINIDOS PELO BACEN
      $polinomio = 0x1021;
      $resultado = 0xFFFF;

      //CHECKSUM
      if (($length = strlen($payload)) > 0) {
          for ($offset = 0; $offset < $length; $offset++) {
              $resultado ^= (ord($payload[$offset]) << 8);
              for ($bitwise = 0; $bitwise < 8; $bitwise++) {
                  if (($resultado <<= 1) & 0x10000) $resultado ^= $polinomio;
                  $resultado &= 0xFFFF;
              }
          }
      }

      //RETORNA CÓDIGO CRC16 DE 4 CARACTERES
      return self::ID_CRC16.'04'.strtoupper(dechex($resultado));
  }
```
___________________

## Canal no YouTube
<img height="60" style="margin-bottom:10px;" src="https://raw.githubusercontent.com/william-costa/william-costa/master/assets/images/logo-wdev.png">

Canal de tecnologia com conteúdos sobre programação e super dicas para a galera que está começando no mundo dev.

Toda **quinta** às **20h** tem vídeo novo, então inscreva-se para não perder nenhuma novidade:

<a href="https://youtube.com/wdevoficial"><img height="30" src="https://raw.githubusercontent.com/william-costa/william-costa/master/assets/images/subscribe-youtube.png"></a>