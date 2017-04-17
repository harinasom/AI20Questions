# AI20Questions
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <time.h>

//setup
void  setupGameData();
void  startGame();
void  askUserQuestion( int, char*, int* );
void  displayRemainingAnimals();
int   countRemainingAnimals();
char* findLastAnimal();

// animal list 
#define ANIMALS_LIST      "Spider Bee Penguin Eagle Giraffe Octopus Tiger Elephant \nJellyfish Bull Parrot Dolphin Python Crocodile Cat \nLeopard Monkey Zebra Sheep Rat Owl Spider \nFrog Polarbear Snail Turtle Rabbit Salmon Rhino Fox"
#define ANIMAL_DELIMITER  " "

//Binary rep of animal type 
#define MAMMALS                    "0 0 0 0 1 0 1 1 0 1 0 1 0 0 1 1 1 1 1 1 0 0 0 1 0 0 1 0 1 1"
#define FLYING_ANIMALS             "1 1 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0"
#define WATER_ANIMALS              "0 0 1 0 0 1 0 0 1 0 0 1 0 1 0 0 0 0 0 0 0 0 1 1 0 1 0 1 1 0"
#define BEAK                       "0 0 1 1 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0"
#define NOCTURNAL_ANIMALS          "0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 0 0 0 0 0 0 0 0 1"
#define SHELL_ANIMALS              "0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 0 0 0 0"
#define SLITHERING_ANIMALS         "0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0"
#define WHISKERS                   "0 0 0 0 0 0 1 0 0 0 0 0 0 0 1 1 0 0 0 1 0 0 0 0 0 0 1 0 0 1"
#define PAWS                       "0 0 0 0 0 0 1 0 0 0 0 0 0 0 1 1 0 0 0 0 0 0 0 1 0 0 1 0 0 1"
#define STRIPES                    "0 1 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0"
#define FUR                        "0 1 0 0 1 0 1 0 0 1 0 0 0 0 1 1 1 1 0 1 0 0 0 1 0 0 1 0 0 1"
#define FOURLEGGED_ANIMALS         "0 0 0 0 1 0 1 1 0 1 0 0 0 1 1 1 1 1 1 1 0 0 1 1 0 1 1 0 1 1"
#define STINGING_ANIMALS           "0 1 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0"
#define TAIL                       "0 0 1 0 1 0 1 1 0 1 0 0 0 1 1 1 1 1 1 1 0 0 0 1 0 0 1 0 1 1"
#define SCALES                     "0 0 0 0 0 0 0 0 0 0 0 0 1 1 0 0 0 0 0 0 0 0 1 0 0 1 0 1 0 0"
#define CARNIVORES                 "0 0 1 1 0 1 1 0 1 0 0 1 1 1 1 1 0 1 0 1 1 1 1 1 0 0 0 0 0 1"
#define SPOTS                      "0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0"
#define GILLS                      "0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0"
#define HORNS                      "0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0"
#define FEATHERS                   "0 0 1 1 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0"

//commands
#define CONTINUE_KEY      "play"
#define YES_KEY           "yes"
#define NO_KEY            "no"
#define EXIT_KEY          "exit"
#define DEBUG_STATUS      0

//sizes
#define TOTAL_ANIMALS     30
#define BUFFER_SIZE       32

//define arrays 
char* animals_list[ TOTAL_ANIMALS ] = { NULL };
int   mammals[ TOTAL_ANIMALS ] = { 0 };
int   flying_animals[ TOTAL_ANIMALS ] = { 0 };
int   water_animals[ TOTAL_ANIMALS ] = { 0 };
int   beak[ TOTAL_ANIMALS ] = { 0 };
int   nocturnal_animals[ TOTAL_ANIMALS ] = { 0 };
int   shell_animals[ TOTAL_ANIMALS ] = { 0 };
int   slithering_animals[ TOTAL_ANIMALS ] = { 0 };
int   whiskers[ TOTAL_ANIMALS ] = { 0 };
int   paws[ TOTAL_ANIMALS ] = { 0 };
int   stripes[ TOTAL_ANIMALS ] = { 0 };
int   fur[ TOTAL_ANIMALS ] = { 0 };
int   fourlegged_animals[ TOTAL_ANIMALS ] = { 0 };
int   stinging_animals[ TOTAL_ANIMALS ] = { 0 };
int   tail[ TOTAL_ANIMALS ] = { 0 };
int   scales[ TOTAL_ANIMALS ] = { 0 };
int   carnivores[ TOTAL_ANIMALS ] = { 0 };
int   spots[ TOTAL_ANIMALS ] = { 0 };
int   gills[ TOTAL_ANIMALS ] = { 0 };
int   horns[ TOTAL_ANIMALS ] = { 0 };
int   feathers[ TOTAL_ANIMALS ] = { 0 };

//starting method 
int main( void )
{

   //intialize values 
   int continue_game = 1;
   int guesses = 0;
   time_t start_time, end_time;
   setupGameData();
   startGame();
   
   //time stamp and time intializing
   start_time = time( NULL );
   printf( "\nGame started at %s\n", ctime( &start_time ) );
   
      //loop begins
      do 
      {
      guesses++;
      switch ( guesses )
         {
             case 1:
      askUserQuestion( guesses, "\nQuestion %d: Is your animal a mammal? \n", mammals );
      break;
     case 2:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal fly?  \n", flying_animals );
      break;
     case 3:
      askUserQuestion( guesses, "\nQuestion %d: Does you your animal live in water only or on land & in water? \n", water_animals );
      break;
     case 4:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have a beak? \n", beak );
      break;
     case 5:
      askUserQuestion( guesses, "\nQuestion %d: Is your animal nocturnal? \n", nocturnal_animals );
      break;
     case 6:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have a shell? \n",shell_animals );
      break;
     case 7:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal slither? \n", slithering_animals );
      break;
     case 8:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have whiskers? \n", whiskers );
      break;
     case 9:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have paws? \n", paws );
      break;
     case 10:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have stripes? \n", stripes );
      break;
     case 11:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have fur? \n", fur );
      break;
     case 12:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have four legs? \n", fourlegged_animals );
      break;
     case 13:
      askUserQuestion( guesses, "\nQuestion %d: Can your animal sting? \n", stinging_animals );
      break;
     case 14:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have a tail? \n", tail );
      break;
     case 15:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have scales? \n", scales );
      break;
     case 16:
      askUserQuestion( guesses, "\nQuestion %d: Is your animal a carnivore? \n", carnivores );
      break;
     case 17:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have patterned spots? \n", spots );
      break;
     case 18:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have gills? \n", gills );
      break;
     case 19:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have horns or tusks \n", horns );
      break;
     case 20:
      askUserQuestion( guesses, "\nQuestion %d: Does your animal have feathers? \n", feathers );
      break;
     default :
      printf( "\nYou won, I cannot guess your animal!\n" );
      // end game 
      continue_game = 0;
      break;
   }

      //consider cases
      if ( DEBUG_STATUS )
           displayRemainingAnimals();
      if ( countRemainingAnimals() == 1 )
         {
           printf( "\nIt took %d attempts to guess your animal, %s!\n", guesses, findLastAnimal() );
           continue_game = 0;
         }
 } while ( continue_game ); //end of do-while loop
 
//end time and time stamp
end_time = time( NULL );
printf( "\nGame ended at %s\n", ctime( &end_time ) );

return 0;
} // end main method 

//list of animal characteristics 
void setupGameData()
{
  char* animal;
  int   index;

  animal = strtok( strdup( ANIMALS_LIST ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   /* Assign the individual animal into the appropriate index */
   animals_list[ index ] = animal;
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( MAMMALS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   mammals[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( FLYING_ANIMALS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   flying_animals[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( WATER_ANIMALS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   water_animals[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }

  animal = strtok( strdup( BEAK ) , ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   beak[ index ] = atoi( animal ); 
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( NOCTURNAL_ANIMALS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   nocturnal_animals[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( SHELL_ANIMALS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   shell_animals[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( SLITHERING_ANIMALS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   slithering_animals[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( WHISKERS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   whiskers[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( PAWS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   paws[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( STRIPES ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   stripes[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( FUR ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   fur[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( FOURLEGGED_ANIMALS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   fourlegged_animals[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( STINGING_ANIMALS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   stinging_animals[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( TAIL ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   tail[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( SCALES ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   scales[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( CARNIVORES ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   carnivores[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( SPOTS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   spots[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( GILLS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   gills[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( HORNS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   horns[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
  animal = strtok( strdup( FEATHERS ), ANIMAL_DELIMITER );
  for ( index = 0; ( index < TOTAL_ANIMALS ) && ( animal != NULL ); index++ )
  {
   feathers[ index ] = atoi( animal );
   animal = strtok( NULL, ANIMAL_DELIMITER );
  }
} //end of animal characteristics 

//Game Setup Screen View
