# Inclusivo · Desempenho do catálogo

Para testar o desempenho de loading dentro do sistema, foram comparadas duas versões da listagem de produtos da Inclusivo. O teste mediu o intervalo entre abrir a loja e exibir o nome do primeiro produto. No cenário com banco remoto, essa espera passou de **12,50 s para 2,57 s**, uma redução de **79,4%** na mediana.

## Resultados

Cada linha compara o mesmo catálogo antes e depois da alteração no backend. Os tempos são medianas de dez aberturas por versão.

| Cenário                                       | Medida                                          |        Antes |      Depois | Redução |
| --------------------------------------------- | ----------------------------------------------- | -----------: | ----------: | ------: |
| Supabase remoto · 24 produtos, 3 vendedores   | Abertura da loja até o primeiro nome de produto | 12.495,95 ms | 2.571,60 ms |   79,4% |
| Supabase remoto · 24 produtos, 3 vendedores   | Requisição do catálogo                          | 10.705,50 ms |   887,50 ms |   91,7% |
| PostgreSQL local · 100 produtos, 5 vendedores | Abertura da loja até o primeiro nome de produto |  2.119,25 ms | 1.928,55 ms |    9,0% |
| PostgreSQL local · 100 produtos, 5 vendedores | Requisição do catálogo                          |    403,55 ms |    45,40 ms |   88,7% |

A requisição ficou mais rápida nos dois cenários. O ganho até a exibição do primeiro nome foi maior com o banco remoto. Como os catálogos e as condições de rede diferiam entre cenários, a comparação relevante é entre as duas versões em cada linha.

## Alteração avaliada

Na versão anterior (`1708fac`), a API fazia consultas por produto para obter dados de reputação e seguidores que a listagem pública omitia. A versão otimizada busca os produtos, os vendedores e os usuários associados em uma única consulta SQL. Um teste de regressão verifica a quantidade de consultas ao listar produtos de vendedores diferentes.

## Como conferir os resultados

- [Amostras com Supabase remoto](./catalog-real-2026-11-15.json)
- [Amostras com PostgreSQL local](./catalog-2026-11-15.json)

Em cada JSON, `before` e `after` identificam as versões anterior e otimizada. Cada entrada de `samples` representa uma abertura da loja e contém duas medidas em milissegundos.

| Campo     | Intervalo medido no navegador                                                                               |
| --------- | ----------------------------------------------------------------------------------------------------------- |
| `cardsMs` | Do início da navegação em `/shop` ao primeiro quadro em que o título de um produto ocupou espaço na página. |
| `apiMs`   | Do início da requisição `GET /api/product/` ao fim do recebimento da resposta.                              |

Para conferir uma mediana, ordene as dez amostras do campo escolhido e calcule a média entre o quinto e o sexto valores. A redução percentual corresponde a `100 × (mediana anterior − mediana posterior) / mediana anterior`. Em ambos os cenários, a quantidade de produtos (`count`) e o SHA-256 do corpo da resposta (`digest`) coincidiram antes e depois da mudança.

## Condições de medição

O Chromium abriu a loja em uma janela de 1440 × 900, com um novo contexto de navegador a cada abertura. As versões usaram o mesmo build de produção do frontend e uma API na máquina de teste. A primeira abertura de cada versão serviu de aquecimento e foi descartada. Recursos externos, como imagens e fontes, foram bloqueados. A medição cobre a exibição do primeiro título nessas condições. O carregamento das fotos, a inicialização a frio e o desempenho do site hospedado ficam fora do seu alcance.
