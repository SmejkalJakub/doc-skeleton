#####################
How to: A/D Converter
#####################

Analog to digital converter can measure the voltage on the one of the six inputs ``A0`` to ``A5`` and return measured value.
The result can be 16 bit value or ``float`` number in volts.

.. tip::

    Visit `documentation for this SDK module <https://sdk.hardwario.com/group__twr__adc.html>`_

***************************
Channels and sampling types
***************************

The ADC subsystem has to be initialized by calling ``twr_adc_init()``.
Each channel can be configured to different resolution and oversampling.
Sampling can be synchronous and asynchronous.
No matter what resolution you choose (6, 8, 10, 12) the result is always scaled to 16 bit value 0-65535.
In asynchronous mode you can also get value directly in volts in the ``float`` data type.

********************
Synchronous sampling
********************

During the synchronous measurement, the code is blocked until the measurement is over.

.. code-block:: c
    :linenos:

    #include <application.h>

    void application_init(void)
    {
        twr_log_init(TWR_LOG_LEVEL_DEBUG, TWR_LOG_TIMESTAMP_OFF);

        twr_adc_init();
    }

    void application_task()
    {
        uint16_t adc;

        twr_adc_get_value(TWR_ADC_CHANNEL_A2, &adc);
        twr_log_debug("%d", adc);

        twr_scheduler_plan_current_relative(200);
    }

*********************
Asynchronous sampling
*********************

Asynchronous sampling is not blocked and is running in the background.
When the result is ready, your callback function is called.
It is possible to start multiple channels, the scheduler samples each channel and calls the callback for each channel separately.

.. code-block:: c
    :linenos:

    #include <application.h>

    static void _adc_event_handler(twr_adc_channel_t channel, twr_adc_event_t event, void *param)
    {
        (void) channel;
        (void) param;

        if (event == TWR_ADC_EVENT_DONE)
        {
            uint16_t adc;
            twr_adc_async_get_value(TWR_ADC_CHANNEL_A2, &adc);
            twr_log_debug("%d", adc);

        //float voltage;
        //twr_adc_get_result_voltage(TWR_ADC_CHANNEL_A2, &voltage);
        //twr_log_debug("%f", voltage);
        }
    }

    void application_init(void)
    {
        twr_log_init(TWR_LOG_LEVEL_DEBUG, TWR_LOG_TIMESTAMP_OFF);

        twr_adc_init();
        twr_adc_set_event_handler(TWR_ADC_CHANNEL_A2, _adc_event_handler, NULL);
        twr_adc_resolution_set(TWR_ADC_CHANNEL_A2, TWR_ADC_RESOLUTION_12_BIT);
        twr_adc_oversampling_set(TWR_ADC_CHANNEL_A2, TWR_ADC_OVERSAMPLING_256);
    }

    void application_task()
    {
        twr_adc_async_measure(TWR_ADC_CHANNEL_A2);

        twr_scheduler_plan_current_relative(200);
    }
