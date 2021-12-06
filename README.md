Javascript to calculate time to submit on each page after click & the difference in time between pageLoad & last Click of radio option

There is also some code at the bottom to resize each radio button size to make it larger and increase the spacing on mobile phones to make it easier to click.

To make sure the time to click the last choice is captured and time to click the next button is captured for a question these 3 steps for code should be in the survey.


1. **Add this code to each radio button select question for which time needs to be captured.**


        /*Calculate Time between Page Load Time and last click Time*/
        Qualtrics.SurveyEngine.addOnload(function()
        {
               /*Page Load Time*/
               var pageLoadTime = Date.now();
               Qualtrics.SurveyEngine.setEmbeddedData('pageLoadTime', pageLoadTime);


              /*Question Id of current question. Need to store this to pass it on page submit*/

              var ID = this.getQuestionInfo().QuestionID;
              Qualtrics.SurveyEngine.setEmbeddedData('ID', ID);

              /*Call the method in header of Look and Feel*/
              applyClick(this, Qualtrics.SurveyEngine.getEmbeddedData('pageLoadTime'));


        });
          Qualtrics.SurveyEngine.addOnReady(function()
          {



          });


        Qualtrics.SurveyEngine.addOnPageSubmit(function()
        {

          /*Place your JavaScript here to run when the page is unloaded*/
          /*Click on Next Button Time*/
          var pageSubmitTime = Date.now();
                   Qualtrics.SurveyEngine.setEmbeddedData('pageSubmitTime', pageSubmitTime);

          /*Call the method in header of Look and Feel and pass the Question ID*/
          nextButtonClick(Qualtrics.SurveyEngine.getEmbeddedData('ID'),pageSubmitTime );



        });


2. **Also please leave this script as is in the Look and Feel - Header section**


        <script>

          /*Apply this onClick function to each radio button to calculate last click time*/
          function applyClick(question, pageLoadTime){

             question.questionclick = function(event, element) {//each time user click on the question
                if(element.type == "radio") { 

                    var ID = this.getQuestionInfo().QuestionID;
                    var clickTime= Date.now();
                    /*Calculate time difference between last click and page Load*/
                    Qualtrics.SurveyEngine.setEmbeddedData('answerTime_' + ID , (clickTime - pageLoadTime));
                    Qualtrics.SurveyEngine.setEmbeddedData('clickTime_' + ID , clickTime );

                }
              }
          }

          function nextButtonClick(ID, pageSubmitTime){

             /*Calculate time difference between next button click and last click of radio button option*/
             Qualtrics.SurveyEngine.setEmbeddedData('submitTime_' + ID , (pageSubmitTime - Qualtrics.SurveyEngine.getEmbeddedData('clickTime_' + ID)));

        }
        </script>
    
    

3. **Add the new embedded data variables in the flow section.**

This is the code to be put in Look and Feel Header section at the bottom to increase size of radio buttons on mobile phones and tablets.

The **alignment of the radio button options should be horizontal** for this to work and not vertical.

        <style type="text/css">@media screen and (max-width: 780px) {
    

                   .Skin .MC .MAHR li, .Skin .MC .MAHR td, .Skin .MC .SAHR li, .Skin .MC .SAHR td{
                        font-size:1.5em;
                        /*To increase the space between each option */
                        margin-bottom:50px;
                        margin-top:50px;
                   }
                }

        </style>
