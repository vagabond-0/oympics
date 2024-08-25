<b>Getting Started</b>
Prerequisites
Java 17 or higher
Maven 3.6.3 or higher
PostgreSQL/MySQL (or any other relational database)
Postman (for testing the endpoints)
Installation
Clone the repository:

bash
Copy code
git clone https://github.com/your-username/olympics-management-system.git
cd olympics-management-system
Configure the database:

Update the application.properties file located in src/main/resources/ with your database configuration.
Example:
properties
Copy code
spring.datasource.url=jdbc:postgresql://localhost:5432/olympicsdb
spring.datasource.username=yourUsername
spring.datasource.password=yourPassword
spring.jpa.hibernate.ddl-auto=update
Build the project:

bash
Copy code
mvn clean install
Run the application:

bash
Copy code
mvn spring-boot:run
First Run Setup: On the first run, the system should automatically register 100 athletes. This can be done through a script or service that seeds the database with initial data. You can create a command line runner or a startup service to achieve this.

Example:

java
Copy code
@Component
public class DataLoader implements CommandLineRunner {

    @Autowired
    private AthleteRepository athleteRepository;

    @Override
    public void run(String... args) throws Exception {
        for (int i = 1; i <= 100; i++) {
            Athlete athlete = new Athlete();
            athlete.setName("Athlete " + i);
            athlete.setCountryCode("Country " + (i % 10 + 1));
            athleteRepository.save(athlete);
        }
    }
}
API Endpoints
Athlete Registration
Endpoint: /register/athlete
Method: POST
Description: Registers a new athlete for a given event.
Request Body:
json
Copy code
{
  "name": "John Doe",
  "countryCode": "USA"
}
Request Param: eventItemId (UUID of the event)
Response: RegistrationEntity object with the athlete and event details.
Medal Tally
Endpoint: /medal-tally/assign-medals
Method: POST
Description: Assigns medals to all events based on athlete scores.
Response: A success message if medals are assigned, or an error message if something goes wrong.
Medal Analysis
Endpoint: /result/medalAnalysis
Method: GET
Description: Provides a summary of the medal tally across all countries.
Response: MedalSummaryDTO with details like highest and lowest medal counts.
Top Athletes
Endpoint: /result/topAthlete

Method: GET

Description: Retrieves the athlete with the highest number of medals.

Response: Athlete object.

Endpoint: /result/topMaleAthlete

Method: GET

Description: Retrieves the top male athlete based on the highest score.

Response: Athlete object.

Endpoint: /result/topFemaleAthlete

Method: GET

Description: Retrieves the top female athlete based on the highest score.

Response: Athlete object.

Top N Medal Tallies
Endpoint: /result/topNMedalTallies/{n}

Method: GET

Description: Retrieves the medal tally of the first N nations that participated.

Path Variable: n (Number of top nations)

Response: List of MedalTally objects.

Endpoint: /result/topNMedalTallies/{eventId}/{n}

Method: GET

Description: Retrieves the medal tally of the first N nations that participated in a given event.

Path Variables:

eventId (UUID of the event)
n (Number of top nations)
Response: List of MedalTally objects.

Event Simulation
Endpoint: /events/simulate-all-events
Method: POST
Description: Simulates scores for all events, generating random scores for each athlete.
Response: List of ScoreEntity objects representing the scores generated for each athlete in each event.
Testing
You can use Postman or any other API testing tool to test the endpoints listed above. Make sure your application is running before sending requests.
