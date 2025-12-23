Neste documento, reunimos os dados completos levantados até o momento para a migração total do **Next** para o **Logos**. Aqui, definiremos inicialmente os *endpoints* e suas regras, considerando os futuros produtos **Logos Cashier**, **Logos POS** e **Logos Admin**, sendo eles:

* **Logos Cashier** → Caixa Next
* **Logos POS** → Next POS
* **Logos Admin** → Next Admin
* **Logos** → Next Hub

> *Os nomes são apenas sugestivos, utilizados para diferenciar os produtos.*

## Importante
---

Alguns pontos precisam ficar claros: a ideia central de todo o projeto é criar um ambiente **totalmente unificado**. Ainda assim, haverá uma divisão de responsabilidades, porém com **um único código e uma única linguagem**, permitindo o compartilhamento de lógicas entre *frontends* ou até entre *frontend* e *backend*, quando fizer sentido.

Isso visa simplificar o desenvolvimento e facilitar a manutenção dos sistemas.

# Requisitos 
---

Os requisitos serão divididos em duas partes:

## [Requisitos Backend](Requisitos Backend.md)

São *endpoints* que serão utilizados pelo *frontend* para executar determinadas operações, como abrir vendas, configurar o servidor, buscar produtos, entre outras.

Também incluem interações entre *backends*, como sincronização (comunicação entre backend local e cloud), download de novas versões, integrações com serviços de delivery, etc.

## [[Requisitos Frontend]Requisitos Frontend.md]

São os requisitos gerais da aplicação, como tela de login, tela de mapa de mesas, botões específicos (X, Y), entre outros.

> [!warning]
> **Não confundir com a ação efetiva**, que é realizada pelo *backend*. 
> Por exemplo: na tela de login, o *frontend* é responsável por exibir o formulário, botões e UX; já a ação de autenticação em si ocorre por meio da interação com o *backend*.


