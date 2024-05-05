# LearningGamification

Esta é uma aplicação Daml para gerenciar inscrições em cursos e acompanhar pontos para cursos concluídos.

## Visão Geral

Esta aplicação consiste em três modelos principais:
- **Application**: Representa uma inscrição em curso enviada por um funcionário.
- **Account**: Controla os pontos ganhos por um funcionário ao concluir cursos.
- **CourseService**: Gerencia operações relacionadas a cursos, como criar uma nova inscrição ou concluir um curso.

## Recursos

- Colaboradores podem enviar inscrições para cursos.
- Colaboradores podem atualizar os detalhes de sua inscrição.
- Colaboradores podem concluir um curso, o que adiciona pontos à sua conta com base no número de horas concluídas.

## DiagramER
   +------------------+      +--------------+       +-----------------+
   |    Employee      |      |    Course    |       |   Application   |
   +------------------+      +--------------+       +-----------------+
   | - employee_id    |      | - course_id  |       | - application_id|
   | - name           |      | - name       |       | - employee_id   |
   | - email          |      | - duration   |       | - course_id     |
   | - phone          |      | - description|       | - name          |
   |                  |      |              |       | - address       |
   |                  |      |              |       | - email         |
   |                  |      |              |       | - phone         |
   |                  |      |              |       | - timestamp     |
   +------------------+      +--------------+       | - status        |
              |                           |         | - score         |
              |                           |         | - comment       |
              |                           |         +-----------------+
              |                           |
              |                           |
              |     +-----------------+   |
              |     |    Account      |   |
              |     +-----------------+   |
              |     | - account_id    |   |
              |     | - employee_id   |   |
              |     | - course_id     |   |
              |     | - points        |   |
              |     +-----------------+   |
              |            |              |
              |            |              |
              |            |              |
              |     +-----------------+   |
              |     | CourseService   |   |
              |     +-----------------+   |
              |     | - service_id    |   |
              |     | - employee_id   |   |
              |     | - course_id     |   |
              |     +-----------------+   |
              +--------------------------+


## Uso

1. Clone o repositório.
2. Instale o Daml SDK.
3. Navegue até o diretório do projeto.
4. Inicie o VSCode: daml studio
5. Inicie o sandbox Daml: daml start.
6. Execute os scripts fornecidos para interagir com a aplicação.

## Testes

*Dentro do Application daml file

## Contribuidores
@triplom

## Lincença
...
