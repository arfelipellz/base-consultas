# Query de Utilização Mensal

## Contatos

```
Thiago Soares
Daniela Leite
Hugo Andreoni
```

## Última atualização
```
16/10/2025
Por: Hugo Andreoni
O que mudou: Retirada das bases do bam, e utilização de master_datas
```

## Query

```sql
SELECT
    Ano
    ,Mes
    ,Dia
    ,Regional
    ,Filial
    ,Agencia
    --,Grupo
    ,NULL AS Segmento
    ,NULL AS `Tipo_de_Contrato`
    ,codigo_faixa_km
    ,Alavanca
    ,Valor
    ,current_date() AS dt_inclusao_registro
    FROM
    (  
      SELECT
    EXTRACT(YEAR FROM b.data_referencia) AS Ano,
    EXTRACT(MONTH FROM b.data_referencia) AS Mes,
    EXTRACT(DAY FROM b.data_referencia) AS Dia,
    t.alavanca AS Alavanca,
    'FI'||RIGHT(LEFT(ag.codigo_filial ,5),3) AS Filial,
    RIGHT(LEFT(ag.codigo_regional,5),3) AS Regional,
    sum(b.quantidade_diarias_mensal) Valor,
    b.codigo_faixa_km AS codigo_faixa_km,
    b.codigo_agencia AS `Agencia`--,
    --m.cd_grupo as Grupo
    FROM
    `lclz-dados.jornada_carro.diarias_situacao_veiculo` b
    LEFT JOIN `lclz-dados.rac_master_data.hierarquia_consolidada` ag  ON (ag.codigo_agencia=b.codigo_agencia)
    --JOIN `lclz-dados.bamdmdcorp.vco_modelo` m on m.cd_modelo=b.cd_modelo
    JOIN
    (
      SELECT '35' AS cd_situacao_veiculo,'Improdutividade Lavagem Interna (35)' AS alavanca
    UNION all SELECT '42','Improdutividade Lavagem Externa (42)'--frota 42
    --UNION all SELECT '35','Frota Improdutiva de Pátio'--lavagem
    --UNION all SELECT '42','Frota Improdutiva de Pátio'--frota 42
    UNION all SELECT '11','Frota Improdutiva de Ativação'--Ativação
    UNION all SELECT '40','Frota Improdutiva de Desativação'--Desativação
    UNION all SELECT '51','Frota Improdutiva de Desativação'--Desativação
    UNION all SELECT '63','Frota Improdutiva de Desativação'--Desativação
    UNION all SELECT '64','Frota Improdutiva de Desativação'--Desativação
    UNION all SELECT '67','Frota Improdutiva de Desativação'--Desativação
    UNION all SELECT '69','Frota Improdutiva de Desativação'--Desativação
    UNION all SELECT '74','Frota Improdutiva de Desativação'--Desativação
   --UNION all SELECT '12','Frota Improdutiva de Pátio'--ag
   --UNION all SELECT '19','Frota Improdutiva de Pátio'--ag
    UNION all SELECT '23','Frota alugada'--Alugada
    UNION all SELECT '21','Frota Disponivel'--Disponivel
    UNION all SELECT '21','Frota da Agência'--Disponivel
    UNION all SELECT '35','Frota da Agência'--lavagem Interna (35)
    UNION all SELECT '12','Frota Improdutiva Trânsito entre agências'--
    UNION all SELECT '19','Frota Improdutiva Trânsito entre agências'--
    UNION all SELECT '24','Frota Improdutiva Manutenção'--
    UNION all SELECT '26','Frota Improdutiva Manutenção'--
    UNION all SELECT '34','Frota Improdutiva Manutenção'--
    UNION all SELECT '36','Frota Improdutiva Manutenção'--
    UNION all SELECT '87','Frota Improdutiva Manutenção'--
    UNION all SELECT '22','Frota Improdutiva Reservado'--reservado
    UNION all SELECT '22','Frota Improdutiva Geral'--reservado
    UNION all SELECT '42','Frota Improdutiva Geral'--Frota 42
    UNION all SELECT '12','Frota Improdutiva Geral'--ag
    UNION all SELECT '19','Frota Improdutiva Geral'--ag
    UNION all SELECT '24','Frota Improdutiva Geral'--Manutencao
    UNION all SELECT '26','Frota Improdutiva Geral'--Manutencao
    UNION all SELECT '36','Frota Improdutiva Geral'--Manutencao
    UNION all SELECT '34','Frota Improdutiva Geral'--Manutencao
    UNION all SELECT '87','Frota Improdutiva Geral'--Manutencao
    UNION all SELECT '41','Frota 41'--Frota 41
    --UNION all SELECT '42','Frota 42'--Frota 42
    )t ON b.codigo_situacao_veiculo = t.cd_situacao_veiculo
 
    WHERE 1=1
    --rg.cd_tipo IN ( '15' )
   --AND EXTRACT(YEAR FROM b.data_referencia)*100+EXTRACT(MONTH FROM b.data_referencia)between EXTRACT(YEAR FROM DATAINI)*100+EXTRACT(MONTH FROM DATAINI) and EXTRACT(YEAR FROM DATAFIM)*100+EXTRACT(MONTH FROM DATAFIM)--SUBSTITUIDO PELA LINHA A SEGUIR O DECLARE
     AND date(b.data_referencia)>= date(date_trunc(date_sub(current_date(),interval 36 month),month))
  AND b.quantidade_diarias_mensal IS NOT null
 
  GROUP BY
  EXTRACT(YEAR FROM b.data_referencia),
  EXTRACT(MONTH FROM b.data_referencia),
  EXTRACT(DAY FROM b.data_referencia),
  t.alavanca,
  b.codigo_agencia,
  Filial,
  ag.codigo_regional,
  b.codigo_faixa_km--,
 
  UNION
 all SELECT
 EXTRACT(YEAR FROM b.data_referencia) AS Ano,
 EXTRACT(MONTH FROM b.data_referencia) AS Mes,
 EXTRACT(DAY FROM b.data_referencia) AS Dia,
 'Frota Operacional RAC',
 'FI'||RIGHT(LEFT(ag.codigo_filial,5),3) AS Filial,
 RIGHT(LEFT(ag.codigo_regional,5),3) AS regional,
 sum(b.quantidade_diarias_mensal) valor,
 b.codigo_faixa_km AS codigo_faixa_km,
 b.codigo_agencia AS agencia--,
 
 
 
 FROM
 `lclz-dados.jornada_carro.diarias_situacao_veiculo` b
 LEFT JOIN `lclz-dados.rac_master_data.hierarquia_consolidada` ag ON (ag.codigo_agencia=b.codigo_agencia)
 --JOIN `lclz-dados.bamdmdcorp.vco_modelo` m on m.cd_modelo=b.cd_modelo
 
 WHERE 1=1
 --rg.cd_tipo IN ( '15' )
 AND
(b.codigo_situacao_veiculo IN ('11','12','19','21','22','23','24','26','34','35','36','40','42','51','57','63','64','67','68','69','74','81','87','A74'))
 --AND EXTRACT(YEAR FROM b.data_referencia)*100+EXTRACT(MONTH FROM b.data_referencia)between EXTRACT(YEAR FROM DATAINI)*100+EXTRACT(MONTH FROM DATAINI) and EXTRACT(YEAR FROM DATAFIM)*100+EXTRACT(MONTH FROM DATAFIM)
 AND date(b.data_referencia)>= date(date_trunc(date_sub(current_date(),interval 36 month),month))
 AND b.quantidade_diarias_mensal IS NOT NULL
 
 GROUP BY
 EXTRACT(YEAR FROM b.data_referencia),
 EXTRACT(MONTH FROM b.data_referencia),
 EXTRACT(DAY FROM b.data_referencia),
 Filial,
 b.codigo_agencia,
 ag.codigo_regional,
 b.codigo_faixa_km--,
 
  )dados
 
 WHERE 1=1
--AND Filial = "FIBHZ"

  /*
  Utilização Geral = Frota Alugada/( Frota operacional Improdutividade)
  Utilização RAC = Frota Alugada/ (Frota Operacioal  - 11 - Desativação)
  Utilização Agência = Frota Alugada/ (Frota alugada+Frota Disponível+35+22)
  Utilização Produtiva = Frota Alugada/ (Frota alugada+Frota Disponível)
  */

```
