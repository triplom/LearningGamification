# LearningGamification

LearningGamification is a Daml project that gamifies the learning process by allowing users to earn points and badges as they complete courses and achieve learning milestones. The project consists of several contract templates, including Application, Account, CourseService, PointsIssuance, PointsCommitment, PointsTransfer, Locking and LockingByChangingState .

## Contract Templates

### Application

The Application template defines the structure of an application, including the parties involved, personal information of the applicant, and the status of the application. The template includes a choice, SubmitApplicationAndSetStatus, which allows the employee to submit the application and set its status.

### Account

The Account template defines the structure of an account, including the parties involved and the number of points associated with the account. The template includes a choice, AddPoints, which allows the employee to add points to the account.

### CourseService

The CourseService template defines the structure of a course service, including the parties involved and the choices available to them. The template includes a non-consuming choice, CreateBlankApplication, which allows the employee to create a blank application. The template also includes a choice, CompleteCourse, which allows the course to complete the course and add points to the account.

### Rewards

This module defines a flexible rewards system where customers can:
Accumulate points.
Transfer points to others.
Exchange points for a variety of assets offered by merchants.
The Rewards.daml module is ideal for loyalty programs and incentive systems that aim to offer a diverse range of rewards to customers.

### AtomicSwaps

The AtomicSwap.daml module defines a comprehensive framework for managing assets and performing atomic swaps. The key features include:
Asset transfer proposals and acceptance.
Asset merging and splitting.
Observer management for assets.
Secure and validated asset swapping between parties.
This module ensures that all asset operations are conducted with proper validations and conditions, making it suitable for scenarios requiring robust asset management and exchange mechanisms.

## Status Data Type

The Status data type defines the possible statuses of an application, including New, InProgress, Pending, Completed, InvalidStatus, and Submitted.

## Main Function

The setup function is the main function that creates a blank application and executes the tests.

## How to Run the Project

To run the project, follow these steps:

1. Clone the repository.
2. Install the Daml SDK.
3. Navigate to the project directory.
4. Start VSCode: `daml studio`
5. Start the Daml sandbox: `daml start`.
6. Execute the provided scripts to interact with the application.

## Exemples
```daml
-- Template Mutual representa a relação de pontos entre o comerciante e o cliente
template Mutual
  with
      merchant: Party  -- Parte representando o comerciante
      customer: Party  -- Parte representando o cliente
      points: Int      -- Pontos que o cliente possui
      assets: [AssetType]  -- Lista de ativos disponíveis para troca
  where
    signatory customer, merchant  -- Cliente e comerciante são signatários

    -- Choice para transferir pontos para um novo proprietário
    choice MutualTransfer: ContractId Mutual
      with
        newOwner: Party  -- Novo proprietário dos pontos
      controller customer
      do
        -- Transfere os pontos para o novo proprietário
        create this with
            customer = newOwner
```
```daml
-- TestRewards.daml

module TestRewards where

import Daml.Script
import Rewards

-- Função auxiliar para alocar partes
allocateParties: Script (Party, Party, Party)
allocateParties = do
    merchant <- allocateParty "Parceiro"
    customer <- allocateParty "Natacha"
    newOwner <- allocateParty "Andre"
    return (merchant, customer, newOwner)

-- Função auxiliar para criar um contrato Mutual
createMutual: Party -> Party -> Int -> Script (ContractId Mutual)
createMutual merchant customer points = do
    submitMulti [customer, merchant] [] do
        createCmd Mutual with
            merchant = merchant
            customer = customer
            points = points
            assets = []
```

## Tests

*Inside the Test* daml files

## Contributors

@triplom @andre

## License

...

## Conclusion

LearningGamification is a Daml project that gamifies the learning process by allowing users to earn points and badges as they complete courses and achieve learning milestones. The project consists of several contract templates, including Application, Account, CourseService, PointsTransferWithAuthorization, PointsIssuance, PointsTransfer, Locking, and LockingByChangingState. These templates facilitate the management of applications, accounts, courses, points transfers, and locking mechanisms within a gamified learning environment.

The project can be run by following the steps outlined above. It provides a comprehensive framework for implementing gamification in learning systems using Daml smart contracts, enabling organizations to incentivize and reward learning achievements effectively.


