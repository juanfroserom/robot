/*
    ChibiOS - Copyright (C) 2006..2016 Giovanni Di Sirio

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
*/

#include "ch.h"
#include "hal.h"
#include "ch_test.h"
#include "chprintf.h"

static void pwmpcb(PWMDriver *pwmp) {

	(void)pwmp;
	//palSetPad(GPIOD, 2);
	palSetPad(GPIOA, 8); // pareja de 10
}

static void pwmc1cb(PWMDriver *pwmp) {

	(void)pwmp;
	//palClearPad(GPIOD, 2);
	palClearPad(GPIOA, 8); // pareja de 10
}

static PWMConfig pwmcfg = {
	10000,                                    /* 10kHz PWM clock frequency.   */
	500,                                    /* Initial PWM period 50 mS.       */
	pwmpcb,
	{
	 {PWM_OUTPUT_ACTIVE_HIGH, pwmc1cb},
	 {PWM_OUTPUT_DISABLED, NULL},
	 {PWM_OUTPUT_DISABLED, NULL},
	 {PWM_OUTPUT_DISABLED, NULL}
	},
	0,
	0,
#if STM32_PWM_USE_ADVANCED
	0
#endif
};

static void pwmp2cb(PWMDriver *pwmp) {

	(void)pwmp;
	palSetPad(GPIOA, 10); // pareja de 14
}

static void pwmc2cb(PWMDriver *pwmp) {

	(void)pwmp;
	palClearPad(GPIOA, 10); // pareja de 14
}



static PWMConfig pwmcfg1 = {
	10000,                                    /* 10kHz PWM clock frequency.   */
	500,                                    /* Initial PWM period 50 mS.       */
	pwmp2cb,
	{
	 {PWM_OUTPUT_ACTIVE_HIGH, pwmc2cb},
	 {PWM_OUTPUT_DISABLED, NULL},
	 {PWM_OUTPUT_DISABLED, NULL},
	 {PWM_OUTPUT_DISABLED, NULL}
	},
	0,
	0,
#if STM32_PWM_USE_ADVANCED
	0
#endif
};


/*Advance

	pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 4000)); 
	pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 6000));
	palSetPad(GPIOA,9);
	palSetPad(GPIOA,11);	
	chThdSleepMilliseconds(1000);
*/

/*Back

	pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 4000)); 
	pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 6000));
	palClearPad(GPIOA,9);
	palClearPad(GPIOA,11);	
	chThdSleepMilliseconds(1000);
*/

/*back_left

	pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 2000)); 
	pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
	palClearPad(GPIOA,9);
	palClearPad(GPIOA,11);
	chThdSleepMilliseconds(500);
*/

/*back_right 

	pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 2000)); 
	pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
	palClearPad(GPIOA,9);
	palClearPad(GPIOA,11);
	chThdSleepMilliseconds(500);
*/

/*Left
	pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 2000)); 
	pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
	palSetPad(GPIOA,9);
	palSetPad(GPIOA,11);
	chThdSleepMilliseconds(500);
*/

/*Right

	pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 2000)); 
	pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
	palSetPad(GPIOA,9);
	palSetPad(GPIOA,11);
	chThdSleepMilliseconds(500);
*/

/*Stop 
	pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
	pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
	palSetPad(GPIOA,9);
	palSetPad(GPIOA,11);
	chThdSleepMilliseconds(1000);
*/



/*
 * Blinker thread #1.
 */
/*
static THD_WORKING_AREA(waThread1, 128);
static THD_FUNCTION(Thread1, arg) {

  (void)arg;

  chRegSetThreadName("blinker");
  while (true) {
    //palSetPad(GPIOC, GPIOC_LED4);
    palSetPad(GPIOD, 2);
    chThdSleepMilliseconds(250);
    //palClearPad(GPIOC, GPIOC_LED4);
    palClearPad(GPIOD, 2);
    chThdSleepMilliseconds(250);
  }
}
*/

/*
 * Blinker thread #2.
 */
static THD_WORKING_AREA(waThread2, 128);
static THD_FUNCTION(Thread2, arg) {

  (void)arg;

  chRegSetThreadName("send2uart");
  while (true) {
	//chprintf((BaseSequentialStream*)&SD1,"comunicacion STM32 \r\n");
	chprintf((BaseSequentialStream*)&SD2,"comunicacion STM32 \r\n");
	//chprintf((BaseSequentialStream*)&SD3,"comunicacion STM32 \r\n");
  chThdSleepMilliseconds(1000);
  }
}

/*
 * Application entry point.
 */

int table=0;
int temp=0;
int PosFx=0;
int PosFy=0;
int PosCx=0;
int PosCy=0;
bool s=true;



int main(void) {



  /*
   * System initializations.
   * - HAL initialization, this also initializes the configured device drivers
   *   and performs the board-specific initializations.
   * - Kernel initialization, the main() function becomes a thread and the
   *   RTOS is active.
   */
  halInit();
  chSysInit();

	/*Pin Config*/
	palSetPadMode(GPIOA, 0, PAL_MODE_INPUT);
	palSetPadMode(GPIOA, 1, PAL_MODE_INPUT);
	palSetPadMode(GPIOA, 2, PAL_MODE_OUTPUT_PUSHPULL);
	palSetPadMode(GPIOA, 3, PAL_MODE_OUTPUT_PUSHPULL);
	palSetPadMode(GPIOA, 4, PAL_MODE_OUTPUT_PUSHPULL);
	palSetPadMode(GPIOA, 5, PAL_MODE_INPUT_PULLDOWN);
	palSetPadMode(GPIOA, 6, PAL_MODE_INPUT_PULLDOWN);
	palSetPadMode(GPIOA, 7, PAL_MODE_INPUT_PULLDOWN);
	palSetPadMode(GPIOA, 8, PAL_MODE_OUTPUT_PUSHPULL); // pwm
	palSetPadMode(GPIOA, 9, PAL_MODE_OUTPUT_PUSHPULL); // digital
	palSetPadMode(GPIOA, 10, PAL_MODE_OUTPUT_PUSHPULL);
	palSetPadMode(GPIOA, 11, PAL_MODE_OUTPUT_PUSHPULL);

	/*PWM*/
	palSetPadMode(GPIOB, 11, PAL_MODE_OUTPUT_PUSHPULL);
	palSetPadMode(GPIOB, 15, PAL_MODE_OUTPUT_PUSHPULL);

	/*Direccion */
	palSetPadMode(GPIOB, 10, PAL_MODE_OUTPUT_PUSHPULL);
	palSetPadMode(GPIOB, 14, PAL_MODE_OUTPUT_PUSHPULL);


	/*PWM_Motor start*/
	pwmStart(&PWMD1, &pwmcfg1);
	pwmEnablePeriodicNotification(&PWMD1);
	pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); //10000/5000 = 50 %
	pwmEnableChannelNotification(&PWMD1, 0);
  
	pwmStart(&PWMD3, &pwmcfg);
	pwmEnablePeriodicNotification(&PWMD3);
	pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
	pwmEnableChannelNotification(&PWMD3, 0);
	/*
   * Activates the serial driver 1 using the driver default configuration.
   * PA9(TX) and PA10(RX) are routed to USART1.
   */
  //sdStart(&SD1, NULL);
  sdStart(&SD2, NULL);
  //sdStart(&SD3, NULL);
  /*
   * Creates the example threads.
   */
  //chThdCreateStatic(waThread1, sizeof(waThread1), NORMALPRIO+1, Thread1, NULL);
  chThdCreateStatic(waThread2, sizeof(waThread2), NORMALPRIO+1, Thread2, NULL);


  /*
   * Normal main() thread activity, in this demo it does nothing except
   * sleeping in a loop and check the button state, when the button is
   * pressed the test procedure is launched.
   */

while(true){

while(table==0){

	palWritePad(GPIOA,2,1);
		if (palReadPad(GPIOA,5) == TRUE){
			table=1;}
		if (palReadPad(GPIOA,6) == TRUE){
			table=2;}
		if (palReadPad(GPIOA,7) == TRUE){
			table=3;}
	palWritePad(GPIOA,2,0);
	palWritePad(GPIOA,3,1);
		if (palReadPad(GPIOA,5) == TRUE){
			table=4;}
		if (palReadPad(GPIOA,6) == TRUE){
			table=5;}
		if (palReadPad(GPIOA,7) == TRUE){
			table=6;}
	palWritePad(GPIOA,3,0);
	palWritePad(GPIOA,4,1);
		if (palReadPad(GPIOA,5) == TRUE){
			table=7;}
		if (palReadPad(GPIOA,6) == TRUE){
			table=8;}
	palWritePad(GPIOA,4,0);


	switch(table){

		case 1:
			PosFx=2;
			PosFy=2;
		break;

		case 2:
			PosFx=4;
			PosFy=2;
		break;

		case 3:
			PosFx=6;
			PosFy=2;
		break;

		case 4:
			PosFx=2;
			PosFy=4;
		break;

		case 5:
			PosFx=4;
			PosFy=4;
		break;

		case 6:
			PosFx=6;
			PosFy=4;
		break;

		case 7:
			PosFx=2;
			PosFy=6;
		break;

		case 8:
			PosFx=4;
			PosFy=6;
		break;
	}

}

while ((PosCx != PosFx) || (PosCy != PosFy)) {
	
	if((PosCx == PosFx) && (PosCy != PosFy)){
		//Stop 
		pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
		pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
		palSetPad(GPIOA,9);
		palSetPad(GPIOA,11);
		chThdSleepMilliseconds(1000);
		//Left
		pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 1000)); 
		pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
		palSetPad(GPIOA,9);
		palSetPad(GPIOA,11);
		chThdSleepMilliseconds(550);
		//Stop 
		pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
		pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
		palSetPad(GPIOA,9);
		palSetPad(GPIOA,11);
		chThdSleepMilliseconds(1000);
		
		while (s){
		if((PosCy == PosFy) && (PosCx == PosFx)){
			//Stop 
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(1000);

			//Left
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 1000)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(550);

			//Stop 
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(1000);

			s=false;
		}
		
		if(PosCy != PosFy){
			if ((palReadPad(GPIOA,0) == TRUE) || (palReadPad(GPIOA,1) == TRUE) ){
				
				if((PosCy+1) == PosFy){
					//Advance
					pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 6000)); 
					pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 6000));
					palSetPad(GPIOA,9);
					palSetPad(GPIOA,11);	
					chThdSleepMilliseconds(1000);
					PosCy=PosCy+1;
					//Stop 
					pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
					pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
					palSetPad(GPIOA,9);
					palSetPad(GPIOA,11);
					chThdSleepMilliseconds(1000);
					//Left
					pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 1000)); 
					pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
					palSetPad(GPIOA,9);
					palSetPad(GPIOA,11);
					chThdSleepMilliseconds(550);

					//Stop 
					pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
					pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
					palSetPad(GPIOA,9);
					palSetPad(GPIOA,11);
					chThdSleepMilliseconds(1000);
				
				s=false;
				}

				if(((PosCy+1) != PosFy) && (PosCy != PosFy)){
					//Advance
					pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 6000)); 
					pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 6000));
					palSetPad(GPIOA,9);
					palSetPad(GPIOA,11);	
					chThdSleepMilliseconds(1500);
					PosCy=PosCy+1;
					//Stop 
					pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
					pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
					palSetPad(GPIOA,9);
					palSetPad(GPIOA,11);
					chThdSleepMilliseconds(1000);
				}

				if((PosCx>PosFx) && (PosCy==PosFy)){
					//Stop 
					pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
					pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
					palSetPad(GPIOA,9);
					palSetPad(GPIOA,11);
					chThdSleepMilliseconds(1000);
					//Left
					pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 1000)); 
					pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
					palSetPad(GPIOA,9);
					palSetPad(GPIOA,11);
					chThdSleepMilliseconds(550);
					//Stop 
					pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
					pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
					palSetPad(GPIOA,9);
					palSetPad(GPIOA,11);
					chThdSleepMilliseconds(1000);
					//Advance
					pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 6000)); 
					pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 6000));
					palSetPad(GPIOA,9);
					palSetPad(GPIOA,11);	
					chThdSleepMilliseconds(1000);
					PosCx=PosCx-1;
					s=false;
				}
				
			}
			
			if ((palReadPad(GPIOA,0) == FALSE) || (palReadPad(GPIOA,1) == FALSE)){
				//Stop 
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(1000);

				//Right
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 1000));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(550);	

				//Stop 
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(1000);

				//Advance
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 6000)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 6000));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);	
				chThdSleepMilliseconds(1500);
				PosCx=PosCx+1;

				//Stop 
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(1000);

				//Left
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 1000)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(550);

				//Stop 
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(1000);
				
				//Advance
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 5000)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 5000));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);	
				chThdSleepMilliseconds(1500);
				PosCy=PosCy+1;

				//Stop 
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(1000);
			
				s=true;	
			}
		}
}
	}
	
	if(PosCx != PosFx){

			if ((palReadPad(GPIOA,0) == TRUE) || (palReadPad(GPIOA,1) == TRUE) ){	
				//Stop 
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(1000);

				//Advance
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 6000)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 6000));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);	
				chThdSleepMilliseconds(1500);
				PosCx=PosCx+1;			
			}

			if ((palReadPad(GPIOA,0) == FALSE) || (palReadPad(GPIOA,1) == FALSE)){
				//Stop 
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(1000);

				//Left
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 1000)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(550);
				
				//Stop 
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);
				chThdSleepMilliseconds(1000);

				//Advance
				pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 6000)); 
				pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 6000));
				palSetPad(GPIOA,9);
				palSetPad(GPIOA,11);	
				chThdSleepMilliseconds(1500);
				PosCy=PosCy+1;
			}	
	}

	
  }
			//Stop 
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(5000);

			//Left
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 1000)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(550);
			
			//Stop 
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(1000);

			//Advance
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 6000)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 6000));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);	
			chThdSleepMilliseconds(1500*PosFy);

			//Stop 
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(1000);

			//Right
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 1000));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(550);

			//Stop 
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(1000);

			//Advance
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 6000)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 6000));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);	
			chThdSleepMilliseconds(1500*PosFx);

			//Stop 
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(1000);

			//Right
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 1000));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(550);

			//Stop 
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(1000);

			//Right
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 1000));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(550);

			//Stop 
			pwmEnableChannel(&PWMD1, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD1, 0)); 
			pwmEnableChannel(&PWMD3, 0, PWM_PERCENTAGE_TO_WIDTH(&PWMD3, 0));
			palSetPad(GPIOA,9);
			palSetPad(GPIOA,11);
			chThdSleepMilliseconds(5000);

			table=0;
	}
}
