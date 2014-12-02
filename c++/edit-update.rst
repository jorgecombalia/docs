.. _cpp_edit_update:


Workflows
==========

Open a block locally to modify and publish a new version of a block.

.. code-block:: bash

  ~/$ bii init myproject
  ~/$ cd myproject
  ~/myproject$ bii open username/blockname


.. container:: infonote

    **A beginner?** check the :ref:`basic guide on opening a block <bii_open_command>`


Opening and editing your dependencies
--------------------------------------

You've built a program and reused your **sum function** in the :ref:`Getting Started <cpp_publish_reuse>`. Now it's time to add new functionality to your published **myuser/math** block, like a **substract function**, and use it in your block **myuser/calc**.

The layout is:

.. code-block:: text

  +-- mycalc
  |    +-- blocks
  |    |    +-- myuser
  |    |    |    +-- calc
  |    |    |    |    +-- main.cpp
  |    +-- deps
  |    |    +-- myuser
  |    |    |    +-- math
  |    |    |    |    +-- operations.cpp
  |    |    |    |    +-- operations.h


Open the block **myuser/math** for editing on the same project, execute:

.. code-block:: bash

  ~/mycalc$ bii open myuser/math

``bii open command`` retrieves the complete block to your ``blocks`` folder, and deletes it from your ``deps`` folder.
In this case, it will open the specific version you depend on. 

The resulting layout is:

.. code-block:: text

  +-- mycalc
  |    +-- blocks
  |    |    +-- myuser
  |    |    |    +-- calc
  |    |    |    |    +-- main.cpp
  |    |    |    +-- math
  |    |    |    |    +-- main.cpp
  |    |    |    |    +-- operations.cpp
  |    |    |    |    +-- operations.h
  |    +-- deps

Now, add the new function, **substract** and use it on your main.cpp

**operations.h**

.. code-block:: cpp

   #pragma once
   int sum(int a, int b);
   int substract(int a, int b);

**operations.cpp**

.. code-block:: cpp

   #include "operations.h"
   int sum(int a, int b) {return a+b;}
   int substract(int a, int b){return a-b;}


**main.cpp**

.. code-block:: cpp

   #include "google/gtest/gtest.h"
   #include "operations.h"
   
   TEST(Sum, Normal) {
    EXPECT_EQ(5, sum(2, 3));
   }
   TEST(Subtract, Normal) {
    EXPECT_EQ(-1, substract(2, 3));
   }
   int main(int argc, char **argv) {
    testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
   }


Build, ``bii cpp:build`` and run your tests ``myuser_math_main`` to check everything is working.


Publishing updated code
-----------------------

Publish the math block again. As you now have 2 blocks opened (calc, math), specify the name of the block you want to publish:

.. code-block:: bash

   ~/mycalc$ bii publish myuser/math

By default, ``bii publish`` uses the DEV tag. Check on your online biicode profile it's been published.

Using ``DEV`` tag, the latest ``DEV`` version is overrided, so **parents.bii** file remains unmodified:

.. code-block:: bash

   # This file contains your block ancestors versions
   * myuser/math: 0


Closing edited block
---------------------

You can now close the **myuser/math** block, it and it will return, with the code already updated, to your ``deps`` folder:

.. code-block:: bash

   ~/mycalc$ bii close myuser/math


Then you can modify the content of your **myuser/calc**:

**main.cpp**

.. code-block:: cpp
   
   #include <iostream>
   #include "myuser/math/operations.h"
   
   using namespace std;
   int main() {
      cout<<"2 + 3 = "<< sum(2, 3)<<endl;
      cout<<"2 - 3 = "<< substract(2,3)<<endl;
   }


and build it, reusing also the new function:

.. code-block:: bash

   ~/mycalc$ bii cpp:build
   ~/mycalc$ bin\myuser_calc_main
   2 + 3 = 5
   2 - 3 = -1

Congrats! You just edited your dependencies and updated the changes. 
You know that we are available at |biicode_forum_link| for any problems.
You can also |biicode_write_us| for suggestions and feedback, they are always welcomed.

.. |biicode_forum_link| raw:: html

   <a href="http://forum.biicode.com" target="_blank">the biicode forum</a>
 

.. |biicode_write_us| raw:: html

   <a href="mailto:info@biicode.com" target="_blank">write us</a>