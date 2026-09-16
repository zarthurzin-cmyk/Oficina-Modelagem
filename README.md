# Oficina Mecânica — Esquema Conceitual de Banco de Dados

Desafio de projeto da DIO.  
Modelei no MySQL Workbench um esquema conceitual EER para controlar ordens de serviço de uma oficina mecânica.

## O que eu fiz

Parti do fluxo da oficina e coloquei a **Ordem de Serviço** no centro do modelo.

Em vez de amarrar veículo na oficina e mecânico no carro, tratei isso como visita:

- o cliente é dono do veículo
- o veículo entra na oficina por meio da OS
- o mecânico trabalha na oficina
- a equipe, os serviços e as peças se ligam à OS daquela visita

Assim o banco guarda o histórico: o mesmo carro pode voltar várias vezes, com equipes e serviços diferentes.

## Como cada parte foi modelada

**Cliente e Veículo**  
Relacionamento 1 para N. Um cliente pode ter vários carros. O veículo não pertence à oficina.

**Oficina**  
A oficina emite as OS e concentra os mecânicos que trabalham nela.

**Mecânicos**  
Cadastro com código, nome, endereço e especialidade.  
Cada mecânico está ligado a uma oficina.  
Não coloquei veículo no mecânico, porque ele atende vários carros ao longo do tempo.

**Equipe**  
Não criei uma entidade Equipe.  
Modelei a equipe como N:N entre OS e Mecânicos (`Ordem de Serviço_has_Mecanicos`).  
Uma OS tem vários mecânicos e um mecânico participa de várias OS.  
A mesma equipe avalia e executa os serviços daquela OS.

**Tipo do atendimento**  
Não criei entidades Revisão e Conserto.  
Isso virou o atributo `Tipo` na OS (`revisao` ou `conserto`).

**Mão de obra**  
Entidade de catálogo com serviço e valor de referência.  
Ligada em N:N com a OS, porque uma OS tem vários serviços e o mesmo serviço entra em várias OS.

**Peça**  
Entidade de catálogo com nome e valor.  
Também N:N com a OS, porque o valor das peças compõe a OS e uma peça pode aparecer em várias ordens.

**Autorização e prazo**  
Na OS coloquei `Autorizado`, `Status`, `Data Emissão` e `DataConclusão`, além de número e valor total.

## Relacionamentos

- Cliente 1 — N Veiculo
- Veiculo 1 — N Ordem de Serviço
- Oficina 1 — N Ordem de Serviço
- Oficina 1 — N Mecanicos
- Ordem de Serviço N — N Mecanicos
- Ordem de Serviço N — N Mão de Obra
- Ordem de Serviço N — N Peça

## Por que o modelo ficou assim

O fato principal do sistema não é o cadastro isolado de carro ou de mecânico.  
É a **visita**: este veículo, nesta oficina, com esta equipe, estes serviços e estas peças, com autorização do cliente e data de conclusão.

Por isso a OS concentra FKs para Veículo e Oficina e se relaciona em N:N com Mecânicos, Mão de Obra e Peça.

## Arquivos

- Mecanico.png — imagem do diagrama EER
