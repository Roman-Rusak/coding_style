
Test C-Style Guide
===============================================


About this guide
----------------
 
Style guide is a set of rules which are aimed to help create readable, maintainable, and robust code. By writing code which looks the same way across the code base we help others read and comprehend the code. By using same conventions for spaces and newlines we reduce chances that future changes will produce huge unreadable diffs. By following common patterns for module structure and by using language features consistently we help others understand code behavior.

C code formatting
-----------------

1.Indentation
-------------

Use 4 spaces for each indentation level. Don't use tabs for indentation. Configure the editor to emit 4 spaces each time you press tab key.

2.Vertical space
----------------

Place one empty line between functions. Don't begin or end a function with an empty line.
::

    void function1(void)
    {
        do_one_thing();
        do_another_thing();
                                    // INCORRECT, don't place empty line here
    }
                                    // place empty line here
    void function2(void)
    {
                                    // INCORRECT, don't use an empty line here
        int var = 0;
        while (var < SOME_CONSTANT) 
        {
            do_stuff(&var);
        }
    }

3.Horizontal space
------------------

Always add single space after conditional and loop keywords( if, switch, case, for, do, while). ::

    if (condition)      // correct
    {   
        // ...
    }

    switch (n)          // correct 
    {        
        case 0:
            // ...
    }

    if (x == y)         // correct
    {       
       // ...
    } 
    else 
    {
       // ...
    }

    for(uint32_t i = 0; i < CONST; ++i)     // INCORRECT
    {    
        // ... 
    }

Don't add single space after these keywords: sizeof, typeof, alignof, or __attribute__. :: 

    s = sizeof(struct file);    // correct

Do not add spaces around (inside) parenthesized expressions. ::
    
    s = sizeof( struct file );  // INCORRECT

Add single space around(on each side of) binary operators and ternary operators. No space is necessary for unary operators::

    const int x = (y != 0U && z > y) ? z : y;               // correct
    
    const int y = y0 + (x - x0) * (y1 - y0) / (x1 - x0);    // correct

    int y_cur = -y;                                         // correct
    ++y_cur;

    const int y = y0+(x-x0)*(y1-y0)/(x1-x0);                // INCORRECT

No space is necessary around ``.`` and ``->`` operators.


Sometimes adding horizontal space within a line can help make code more readable. For example, you can add space to align function arguments::

    gpio_matrix_in(PIN_CAM_D6,   I2S0I_DATA_IN14_IDX, false);
    gpio_matrix_in(PIN_CAM_D7,   I2S0I_DATA_IN15_IDX, false);
    gpio_matrix_in(PIN_CAM_HREF, I2S0I_H_ENABLE_IDX,  false);
    gpio_matrix_in(PIN_CAM_PCLK, I2S0I_DATA_IN15_IDX, false);

Note however that if someone goes to add new line with a longer identifier as first argument (e.g.  ``PIN_CAM_VSYNC``), it will not fit. So other lines would have to be realigned, adding meaningless changes to the commit. 

Therefore, use horizontal alignment sparingly, especially if you expect new lines to be added to the list later.

Never use TAB characters for horizontal alignment.

Never add trailing whitespace at the end of the line.


4.Braces
--------

- Function definition should have a brace on a separate line::

    // This is correct:
    void function(uint32_t arg)
    {
        // ...
    }

    // NOT like this:
    void function(uint32_t arg) {
        // ...
    }

- Conditional and loop statements should have a brace on a separate line::
    
    if (condition) 
    {
        do_one();
    } 
    else if (other_condition) 
    {
        do_two();
    }

5.Naming
--------

GLOBAL variables and functions(to be used only if you really need them) need to have descriptive names and should be always ``lower_case``.
The global function name must contain the name of the module in which it is defined.
If you have a function that get ADC value and defined in ``bsp/bsp_adc.c``, you should call that: 
::

    uint32_t bsp_adc_get_value(void);                // correct

    uint32_t bsp_get_adc_value(void);                // INCORRECT

    uint32_t adc_get_value(void);                    // INCORRECT

You can shorten function names, the preferred name length is less than 40 symbols.
If you have a function that counts the number of active users and defined in ``app/statistics/statistics.c``, you can call that:
::
    uint32_t statistics_count_active_users(void);    // correct 

    uint32_t stat_count_active_users(void);          // also correct

    uint32_t cntusr(void);                           // INCORRECT  

Encoding the type of a function into the name (so-called Hungarian
notation) is brain damaged - the compiler knows the types anyway and can
check those, and it only confuses the programmer.

LOCAL function name should be short but descriptive, and start with  ``_`` prefix.
::

    static uint32_t _usr_counter(void)                          // correct
    {
        // ... 
    }

    static uint32_t _this_function_return_user_counter(void)    // INCORRECT
    {
        // ... 
    }  

    static uint32_t usr_counter(void)                           // INCORRECT
    {
        // ... 
    }

    static uint32_t _foo(void)                                  // INCORRECT
    {
        // ...
    }

LOCAL variable names declared as static within a file should be descriptive, and start with  ``_`` prefix.
::
    static bool _is_ack_received = false;   // correct
    static bool _is_ack = false;            // also correct
    static bool is_ack_received = false;    // INCORRECT
    static bool _flag1 = false;             // INCORRECT

LOCAL variable names should be short, and to the point.  If you have
some random integer loop counter, it should probably be called ``i``.
Calling it ``loop_counter`` is non-productive, if there is no chance of it
being mis-understood.  Similarly, ``tmp`` can be just about any type of
variable that is used to hold a temporary value.

CONST variable names should be always UPPERCASE.::

    const uint32_t DAYS_IN_WEEK = 7U;       // correct
    const uint32_t days_in_week = 7U;       // INCORRECT

DEFINE statements and macros names should be always UPPERCASE.::

    #define SEC_PER_YEAR         (60U * 60U * 24U * 365UL)     // correct
    #define MESSAGE_BUFFER_SIZE  (512U)                        // correct
    #define MIN(x,y)             (((x) < (y)) ? (x) : (y))     // correct
    #define min(x,y)             (((x) < (y)) ? (x) : (y))     // INCORRECT 

6.Enum
-------

It's preferable to use ``enum`` instead ``#define`` for multiple definition. Enum should have a brace on a separate line.
Don't begin or end an ``enum`` with an empty line. Enum members must be written in a column.
::

    enum example_e                          // correct 
    {   
        ELM_1,
        ELM_2,
        ELM_3 
    };

    enum example_e 
    {      
                                            // INCORRECT, don't place empty line here
        ELM_1,
        ELM_2,
        ELM_3 
    };

     enum example_e
    {    
        ELM_1,
        ELM_2,
        ELM_3
                                            // INCORRECT, don't place empty line here 
    };


    enum example_e {ELM_1, ELM_2, ELM_3};   // INCORRECT

Enum should have descriptive name and the name must end with the ``_e`` postfix.
Enum member names should be always UPPERCASE and must contain at least part of the ``enum`` name.
::
    enum gnss_mode_e                        // correct 
    {               
        GNSS_MODE_GPS,
        GNSS_MODE_SBAS,
        GNSS_MODE_GALILEO,
        GNSS_MODE_BEIDOU,
        GNSS_MODE_IMES,
        GNSS_MODE_QZSS,
        GNSS_MODE_GLONASS
    };

    enum gnss_mode_e                        // also correct
    {                
        MODE_GPS,
        MODE_SBAS,
        MODE_GALILEO,
        MODE_BEIDOU,
        MODE_IMES,
        MODE_QZSS,
        MODE_GLONASS
    };

    enum gnss_mode_e                         // INCORRECT  
    {              
        GPS,
        SBAS,
        GALILEO,
        BEIDOU,
        IMES,
        QZSS,
        GLONASS
    } 

7.Struct
---------

Struct should have a brace on a separate line. Don't begin or end a ``struct`` with an empty line.
::
    struct sample_s                         // correct
    {                       
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        uint8_t fourth_field;
        bool sample_flag;
    };

    struct sample_s {                        // INCORRECT 
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        uint8_t fourth_field;
        bool sample_flag;
    };

    struct sample_s
    {       
                                            // INCORRECT, don't place empty line here                 
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        uint8_t fourth_field;
        bool sample_flag;
    };

Struct should have descriptive name and the name must end with the ``_s`` postfix.
::
    struct sample_s                         // correct
    {                       
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        uint8_t fourth_field;
        bool sample_flag;
    };

    struct sample                           // INCORRECT
    {                          
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        uint8_t fourth_field;
        bool sample_flag;
    };

Struct members should be always ``lower_case`` and written in a column.
::

    struct sample_s                         // correct
    {                       
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        uint8_t fourth_field;
        bool sample_flag;
    };

    struct sample_s                         // INCORRECT
    {                       
        uint32_t FirstField;
        uint8_t SecondField;
        uint8_t ThirdField;
        uint8_t FourthField;
        bool SampleFlag;
    };

    struct sample_s { uint32_t first_field; uint8_t second_field; uint8_t third_field; };  // INCORRECT

8.Typedef
---------

Typedef should have a brace on a separate line. Don't begin or end a ``typedef`` with an empty line.
::
    typedef struct sample_s                 // correct
    {                       
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        uint8_t fourth_field;
        bool sample_flag;
    } sample_t;

    typedef struct sample_s {               // INCORRECT 
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        uint8_t fourth_field;
        bool sample_flag;
    } sample_t;

    typedef struct sample_s
    {       
                                            // INCORRECT, don't place empty line here                 
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        uint8_t fourth_field;
        bool sample_flag;
    } sample_t;

Typedef should have descriptive name and the name must end with the ``_t`` postfix.
Add single space between closing brace and ``typedef`` name.
::
    typedef enum gnss_mode_e
    {                      
        MODE_GPS = 0U,
        MODE_SBAS,
        MODE_GALILEO,
        MODE_BEIDOU,
        MODE_IMES,
        MODE_QZSS,
        MODE_GLONASS
    } gnss_mode_t;               // correct 

    typedef enum gnss_mode_e
    {                      
        MODE_GPS = 0U,
        MODE_SBAS,
        MODE_GALILEO,
        MODE_BEIDOU,
        MODE_IMES,
        MODE_QZSS,
        MODE_GLONASS
    }gnss_mode_t;                // INCORRECT

    typedef enum gnss_mode_e
    {                      
        MODE_GPS = 0U,
        MODE_SBAS,
        MODE_GALILEO,
        MODE_BEIDOU,
        MODE_IMES,
        MODE_QZSS,
        MODE_GLONASS
    } gnss_mode;                // INCORRECT

9.Functions
-----------

Functions should be short and sweet, and do just one thing.  They should
fit on one or two screenfuls of text (the ISO/ANSI screen size is 80x24,
as we all know), and do one thing and do that well.

The maximum length of a function is inversely proportional to the
complexity and indentation level of that function.  So, if you have a
conceptually simple function that is just one long (but simple)
case-statement, where you have to do lots of small things for a lot of
different cases, it's OK to have a longer function.

However, if you have a complex function, and you suspect that a
less-than-gifted first-year high-school student might not even
understand what the function is all about, you should adhere to the
maximum limits all the more closely.  Use helper functions with
descriptive names (you can ask the compiler to in-line them if you think
it's performance-critical, and it will probably do a better job of it
than you would have done).

Another measure of the function is the number of local variables.  They
shouldn't exceed 5-10, or you're doing something wrong.  Re-think the
function, and split it into smaller pieces.  A human brain can
generally easily keep track of about 7 different things, anything more
and it gets confused.

10.Function arguments
---------------------

- All arguments passed by value and do not modified in function must be labeled ``const``.
::

    void some_function(const uint32_t ext_arg)      // correct
    {
        static uint32_t sample_arg = 0U;

        if(sample_arg != ext_arg) 
        {
            do_something();
            sample_arg = ext_arg;
        }
    }

    void some_function(uint32_t ext_arg)            // INCORRECT
    {
        static uint32_t sample_arg = 0U;

        if(sample_arg != ext_arg) 
        {
            do_something();
            sample_arg = ext_arg;
        }
    }

- All pointers to arguments passed to function and do not modified in function must be labeled ``const``.
::

    bool check_settings(settings_t* const settings) // correct
    {
        //...     
        if(settings != NULL) 
        {
            return true;
        }
        return false;
    }

    bool check_settings(settings_t* settings)       // INCORRECT
    {
        //...
        if(settings != NULL) 
        {
            return true;
        }
        return false;
    }

- Do not pass to much arguments to function, use ``struct``, ``typedef`` instead.
::
    
    // INCORRECT 
    void some_function(const uint8_t *const a, const uint32_t b, const uint32_t c, 
                        const uint32_t d, const uint32_t e, const uint32_t f,
                        const uint32_t x, const uint32_t y, const uint32_t z)         
    {
        // ... 
    }   


11.Comments
-----------

Use ``//`` for single line comments. For multi-line comments it is okay to use either ``//`` on each line or a ``/* */`` block.

Although not directly related to formatting, here are a few notes about using comments effectively.

- Don't use single comments to disable some functionality::

    void init_something(void)
    {
        setup_dma();
        // load_resources();                // WHY is this thing commented, asks the reader?
        start_timer();
    }

- If some code is no longer required, remove it completely. If you need it you can always look it up in git history of this file. If you disable some call because of temporary reasons, with an intention to restore it in the future, add explanation on the adjacent line::

    void init_something(void)
    {
        setup_dma();
        // TODO: we should load resources here, but loader is not fully integrated yet.
        // load_resources();
        start_timer();
    }

- Same goes for ``#if 0 ... #endif`` blocks. Remove code block completely if it is not used. Otherwise, add comment explaining why the block is disabled. Don't use ``#if 0 ... #endif`` or comments to store code snippets which you may need in the future.

- Don't add trivial comments about authorship and change date. You can always look up who modified any given line using git. E.g. this comment adds clutter to the code without adding any useful information::

    void init_something(void)
    {
        setup_dma();
        // XXX add 2016-09-01
        init_dma_list();
        fill_dma_item(0);
        // end XXX add
        start_timer();
    }

The preferred style for long (multi-line) comments is: ::

    /*
     * This is the preferred style for multi-line
     * comments.
     * Please use it consistently.
     *
     * Description:  A column of asterisks on the left side,
     * with beginning and ending almost-blank lines.
     */

12.Breaking long lines and strings
----------------------------------

The limit on the length of lines is 80 columns and this is a strongly preferred limit.
Statements longer than 80 columns will be broken into sensible chunks, unless exceeding 80 columns significantly increases 
readability and does not hide information. Descendants are always substantially shorter than the parent and are placed substantially to the right. 
The same applies to function headers with a long argument list. 
However, never break user-visible strings such as printk messages, because that breaks the ability to grep for them. 
:: 
    // This is correct
    void some_function(const uint8_t* const x, const uint32_t y,
                        const uint32_t z, bool* const q)               
    {
        // ...
    }                        

    // This is also correct
    void some_function_with_a_very_long_name(
                        const uint8_t* const x, const uint32_t y, 
                        const uint32_t z, bool* const q) 
    {
        // ...
    }

    // INCORRECT 
    void some_function(const uint8_t* const x, const uint32_t y, const uint32_t z, bool* const q)         
    {
        // ... 
    }                        

12.File structure
-------------------

In ``.c`` files you must follow the next file structure:
::
    /* ===== INCLUDES =========================================================== */

    #include <string.h>

    /* ===== DEFINE ============================================================= */

    #define MIN(x,y)             (((x) < (y)) ? (x) : (y))

    /* ===== ENUMS ============================================================== */

    enum sample_e                           
    {   
        SAMPLE_FIELD_1,
        SAMPLE_FIELD_2,
        SAMPLE_FIELD_3 
    };

    /* ===== TYPES ============================================================== */

    typedef struct sample_s                 
    {                       
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        bool sample_flag;
    } sample_t;

    /* ===== STRUCTURES ========================================================= */

    struct some_struct_s                         
    {                       
        uint32_t first_field;
        uint8_t second_field;
        bool sample_flag;
    };

    /* ===== LOCAL FUNCTIONS PROTOTYPES ========================================= */

    static uint32_t _some_function(void); 

    /* ===== LOCAL VARIABLES ==================================================== */

    static bool _is_whatever = true;

    /* ===== GLOBAL FUNCTIONS =================================================== */

    void some_task(void)
    {
        //...
    }

    /* ===== LOCAL FUNCTIONS ==================================================== */

    static uint32_t _some_function(void)
    {
        //...
    }

In ``.h`` files you must follow the next file structure:
::
    /* ===== INCLUDES =========================================================== */

    #include <string.h>

    /* ===== DEFINE ============================================================= */

    #define MIN(x,y)             (((x) < (y)) ? (x) : (y))

    /* ===== ENUMS ============================================================== */

    enum sample_e                           
    {   
        SAMPLE_FIELD_1,
        SAMPLE_FIELD_2,
        SAMPLE_FIELD_3 
    };

    /* ===== TYPES ============================================================== */

    typedef struct sample_s                 
    {                       
        uint32_t first_field;
        uint8_t second_field;
        uint8_t third_field;
        bool sample_flag;
    } sample_t;

    /* ===== GLOBAL FUNCTIONS PROTOTYPES ======================================== */

    void some_task(void);

Formatting your code
^^^^^^^^^^^^^^^^^^^^

You can use ``astyle`` program to format your code according to the above recommendations.


