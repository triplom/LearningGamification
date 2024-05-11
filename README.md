# LearningGamification

LearningGamification is a Daml project that gamifies the learning process by allowing users to earn points and badges as they complete courses and achieve learning milestones. The project consists of several contract templates, including Application, Account, CourseService, and PointsTransferWithAuthorization.

## Contract Templates

### Application

The Application template defines the structure of an application, including the parties involved, personal information of the applicant, and the status of the application. The template includes a choice, SubmitApplicationAndSetStatus, which allows the employee to submit the application and set its status.

### Account

The Account template defines the structure of an account, including the parties involved and the number of points associated with the account. The template includes a choice, AddPoints, which allows the employee to add points to the account.

### CourseService

The CourseService template defines the structure of a course service, including the parties involved and the choices available to them. The template includes a non-consuming choice, CreateBlankApplication, which allows the employee to create a blank application. The template also includes a choice, CompleteCourse, which allows the course to complete the course and add points to the account.

### PointsTransferWithAuthorization

The PointsTransferWithAuthorization template defines the structure of a points transfer, including the sender, receiver, points, status, and authorization. The template includes several choices, including RequestPointsTransfer, AuthorizePointsTransfer, CompletePointsTransfer, and RejectPointsTransfer, which allow the sender and receiver to request, authorize, complete, and reject points transfers, respectively.

## Status Data Type

The Status data type defines the possible statuses of an application, including New, InProgress, Pending, Completed, InvalidStatus, and Submitted.

## Main Function

The setup function is the main function that creates a blank application and executes the tests.

## How to Run the Project

To run the project, follow these steps:

1. Install Daml SDK
2. Clone the repository
3. Run `daml build`
4. Run `daml start`

## Conclusion

LearningGamification is a Daml project that gamifies the learning process by allowing users to earn points and badges as they complete courses and achieve learning milestones. The project consists of several contract templates, including Application, Account, CourseService, and PointsTransferWithAuthorization. The project can be run by following the steps outlined above.

## Usage

1. Clone the repository.
2. Install the Daml SDK.
3. Navigate to the project directory.
4. Start VSCode: `daml studio`
5. Start the Daml sandbox: `daml start`.
6. Execute the provided scripts to interact with the application.

## Tests

*Inside the Application daml file

## Contributors

@triplom @andre

## License

...

