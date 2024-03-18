Second ReadMe File:

Unit Testing:

java
package atmsystem;

public class ATMSystemInfoTest {

    // Mock class that implements ATMSystemInfo for testing
    public class ATMSystem implements ATMSystemInfo {
        private boolean started;

        @Override
        public void startATM() {
            started = true;
        }

        public boolean isStarted() {
            return started;
        }
    }

    @Test
    public void testStartATM() {
        ATMSystem ATMSystem = new ATMSystem();
        assertFalse(ATMSystem.isStarted()); // ATM should not be started initially

        ATMSystem.startATM(); // Start the ATM
        assertTrue(ATMSystem.isStarted()); // ATM should be started now
    }
}

We conducted many tests on how to get the system functioning. Our first plan of attack was creating all of the functions and use cases we listed in our system functions document.

Then integrating the additional features the user can utilize which are login, main menu, deposit, withdraw, transfer, and view account balance. 

We also implemented accessibility features such as a the language feature as well as the 'exit' or logout function of the system. This was one of the first tests we implemented.