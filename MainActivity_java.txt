package com.example.android.courtcounter;

import android.app.Activity;
import android.os.CountDownTimer;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.view.animation.AlphaAnimation;
import android.view.animation.Animation;
import android.view.animation.LinearInterpolator;
import android.widget.Button;
import android.widget.TextView;

public class MainActivity extends Activity {

    /*
     * variables for score
     */
    int team_a_score = 0;
    int team_b_score = 0;

    /*
     * variables for statistics
     */
    int threeThrowquantityA = 0;
    int threeThrowquantityB = 0;
    int twoThrowquantityA = 0;
    int twoThrowquantityB = 0;
    int oneThrowquantityA = 0;
    int oneThrowquantityB = 0;

    /*
     * variables for countDownTimer
     */
    private TextView countdownText;
    private Button countdownButton;

    private CountDownTimer contDownTimer;
    private long timeLeftInMilliseconds = 600000; //10 minuts
    private boolean timeRunning; //false

    /*
     * variables for flashing
     */
    private View mainBackground;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.activity_main);

        countdownText = findViewById(R.id.countdown_text);                      //find TextView
        countdownButton = findViewById(R.id.countdown_button);                  //find button Start Timer

        mainBackground = findViewById(R.id.viewBackground);                     //find background view

        countdownButton.setOnClickListener(new View.OnClickListener() {         //when the button is clicked
            @Override
            public void onClick(View view) {                                    //on click
                startStop();                                                    //go to startStop
            }
        });
        updateTimer();                                                          //refresh view
    }

    /*
     * --------------------------------------------------------------------------------
     * COUNTDOWN TIMER
     * --------------------------------------------------------------------------------
     */

    public void startStop() {
        if (timeRunning) {                                                      //if time is running
            stopTimer();                                                        //stop countdown
        } else {                                                                //if time isn't running
            startTimer();                                                       //start countdown

        }

    }

    public void startTimer() {
        contDownTimer = new CountDownTimer(timeLeftInMilliseconds, 1000) {          //start timer, schedule a countdown with regular intervals
            @Override
            public void onTick(long l) {
                timeLeftInMilliseconds = l;                                     //it's a letter not a number, callback fired on regular interval.
                updateTimer();                                                  //refresh view
            }

            @Override
            public void onFinish() {
                timeLeftInMilliseconds = 600000;                               //restart timer
                countdownButton.setText(R.string.countdown_Start_btn);        //change name of button
                timeRunning = false;
                updateTimer();
                mainBackground.startAnimation(getBlinkAnimation());            //flashing
            }
        }.start();

        countdownButton.setText(R.string.countdown_Pause_btn);                //start time and rename button to "pause"
        timeRunning = true;

    }

    public void stopTimer() {                                                   //stop timer
        contDownTimer.cancel();
        countdownButton.setText(R.string.countdown_Start_btn);                  //stop time and rename button to "start"
        timeRunning = false;
    }

    public void updateTimer() {                                                 //determine how the time is displayed
        int minutes = (int) timeLeftInMilliseconds / 60000;                     //convert milliseconds to minutes
        int seconds = (int) timeLeftInMilliseconds % 60000 / 1000;              //convert rest of milliseconds to seconds (the rest of the division - modulo - %)

        String timeLeftText;                                                    //format text of time

        timeLeftText = "" + minutes;
        timeLeftText = timeLeftText + ":";
        if (seconds < 10) timeLeftText = timeLeftText + "0";                    //display "9" seconds as "09"
        timeLeftText = timeLeftText + seconds;

        countdownText.setText(timeLeftText);


    }
    /*
     * -----------------------------------------------------------------------------
     * FLASHING
     * -----------------------------------------------------------------------------
     */
    public Animation getBlinkAnimation() {
        Animation animation = new AlphaAnimation(1, 0);                         // Change alpha from fully visible to invisible
        animation.setDuration(300);                                             // duration - half a second
        animation.setInterpolator(new LinearInterpolator());                    // do not alter animation rate
        animation.setRepeatCount(5);                                            // Repeat animation infinitely
        animation.setRepeatMode(Animation.REVERSE);                             // Reverse animation at the end so the button will fade back in
        return animation;

    }
    /*
      -----------------------------------------------------------------------------
      SCORE
      -----------------------------------------------------------------------------
     */

    /*
     * This method is called when the +3 button for team A is clicked.
     */
    public void addThreeForTeamA(View view) {
        team_a_score = team_a_score + 3;
        displayForTeamA(team_a_score);
        threeThrowquantityA = threeThrowquantityA + 1;
        displayquantityThreeThrowA(threeThrowquantityA);
    }

    /*
     * This method is called when the +2 button for team A  is clicked.
     */
    public void addTwoForTeamA(View view) {
        team_a_score = team_a_score + 2;
        displayForTeamA(team_a_score);
        twoThrowquantityA = twoThrowquantityA + 1;
        displayquantityTwoThrowA(twoThrowquantityA);
    }

    /*
     * This method is called when the ThreeThrow button for team A  is clicked.
     */
    public void addOneForTeamA(View view) {
        team_a_score = team_a_score + 1;
        displayForTeamA(team_a_score);
        oneThrowquantityA = oneThrowquantityA + 1;
        displayquantityOneThrowA(oneThrowquantityA);
    }

    /*
     * This method is called when the +3 button for team B is clicked.
     */
    public void addThreeForTeamB(View view) {
        team_b_score = team_b_score + 3;
        displayForTeamB(team_b_score);
        threeThrowquantityB = threeThrowquantityB + 1;
        displayquantityThreeThrowB(threeThrowquantityB);
    }

    /*
     * This method is called when the +2 button for team B  is clicked.
     */
    public void addTwoForTeamB(View view) {
        team_b_score = team_b_score + 2;
        displayForTeamB(team_b_score);
        twoThrowquantityB = twoThrowquantityB + 1;
        displayquantityTwoThrowB(twoThrowquantityB);
    }

    /*
     * This method is called when the ThreeThrow button for team B  is clicked.
     */
    public void addOneForTeamB(View view) {
        team_b_score = team_b_score + 1;
        displayForTeamB(team_b_score);
        oneThrowquantityB = oneThrowquantityB + 1;
        displayquantityOneThrowB(oneThrowquantityB);
    }

    /*
     * Displays the given score for Team A.
     */
    public void displayForTeamA(int scoreTeamA) {
        TextView scoreView = findViewById(R.id.text_team_a_score);
        scoreView.setText(String.valueOf(scoreTeamA));
    }

    /*
     * Displays the given score for Team B.
     */
    public void displayForTeamB(int scoreTeamB) {
        TextView scoreView = findViewById(R.id.text_team_b_score);
        scoreView.setText(String.valueOf(scoreTeamB));
    }

    /*
     * --------------------------------------------------------------------------
     * STATISTICS
     * --------------------------------------------------------------------------
     */


    /*
     * Displays the number of throws for three points for team A.
     */
    public void displayquantityThreeThrowA(int quantityThreeA) {
        TextView quantityView = findViewById(R.id.ThreeThrowValueA);
        quantityView.setText(String.valueOf(quantityThreeA));
    }

    /*
     * Displays the number of throws for two points for team A.
     */
    public void displayquantityTwoThrowA(int quantityTwoA) {
        TextView quantityView = findViewById(R.id.TwoThrowValueA);
        quantityView.setText(String.valueOf(quantityTwoA));
    }

    /*
     * Displays the number of throws for one point for team A.
     */
    public void displayquantityOneThrowA(int quantityOneA) {
        TextView quantityView = findViewById(R.id.OneThrowValueA);
        quantityView.setText(String.valueOf(quantityOneA));
    }

    /*
     * Displays the number of throws for three points for team B.
     */
    public void displayquantityThreeThrowB(int quantityThreeB) {
        TextView quantityView = findViewById(R.id.ThreeThrowValueB);
        quantityView.setText(String.valueOf(quantityThreeB));
    }

    /*
     * Displays the number of throws for two points for team B.
     */
    public void displayquantityTwoThrowB(int quantityTwoB) {
        TextView quantityView = findViewById(R.id.TwoThrowValueB);
        quantityView.setText(String.valueOf(quantityTwoB));
    }

    /*
     * Displays the number of throws for one point for team B.
     */
    public void displayquantityOneThrowB(int quantityOneB) {
        TextView quantityView = findViewById(R.id.OneThrowValueB);
        quantityView.setText(String.valueOf(quantityOneB));
    }
/*
 * --------------------------------------------------------------------------
 * RESET
 * --------------------------------------------------------------------------
 */
    /*
     * This method is called when the Reset button is clicked.
     */
    public void resetScore(View view) {
        /*
         * score reset
         */
        team_a_score = 0;
        team_b_score = 0;
        /*
         * statistics reset
         */
        threeThrowquantityA = 0;
        threeThrowquantityB = 0;
        twoThrowquantityA = 0;
        twoThrowquantityB = 0;
        oneThrowquantityA = 0;
        oneThrowquantityB = 0;
        /*
         * display reseted score
         */
        displayForTeamA(team_a_score);
        displayForTeamB(team_b_score);
        /*
         * display reseted statistics
         */
        displayquantityThreeThrowA(threeThrowquantityA);
        displayquantityTwoThrowA(twoThrowquantityA);
        displayquantityOneThrowA(oneThrowquantityA);
        displayquantityThreeThrowB(threeThrowquantityB);
        displayquantityTwoThrowB(twoThrowquantityB);
        displayquantityOneThrowB(oneThrowquantityB);
    }

}
