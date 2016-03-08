# Introduction #

The guidelines that will follow should be followed when making commits and changes to the RootTools library. If there are any questions regarding these guidelines please contact either Stericson or ChainsDD.


# Stable VS Developmental #

  * When making changes to the RootTools library it is very important  that we are only making changes to the code within the developmental folder.
    * (This keeps the production ready code clean for anyone wanting to download it to their machine.)

  * The only people that should be making changes to the code within the stable folder is ChainsDD, Stericson and CyanSmoker.
    * _Any commit to this folder by anyone else will be reverted and may result in that developer being removed from the project as a contributor._

# Branching #

  * We do encourage the use of branching.
    * If you feel the need to create your own branch off of the library you are working with within the developmental folder then please feel free to do so.

# Creating new Libraries #

  * If you would like to create a new library, maybe one compiled for a different version of the Android SDK, then you are welcome to do so.
    * When creating a new folder within the developmental folder please be aware that we follow the following naming schematic: **`RootTools_[sdk version]_[generic or specific to?]`** _Example: RootTools\_sdk3\_generic_

# Examples #

  * If you would like to provide working examples on using the library, or using a specific method in the library then we would prefer for you to put them here. Each library that we offer should have their own folder within the Examples folder. For the time being, we only offer one library which is compiled against sdk3. If you had examples for the library compiled against sdk3 then you would put them in /Examples/RootTools\_sdk3\_generic.

  * You are welcome to create a private folder, within the targeted library folder, containing your example project.

# Changing/Modifying/Removing/Deprecating other developers code/methods #

  * If you are working within a project and you see the need to edit, remove, deprecate, or otherwise change another developers code we strongly urge you to discuss your changes with them before doing so.

  * We do not want other developers hard work being erased or changed without their knowledge because this could lead to negative feelings, animosity, or other feelings of discontent if the developer of the original code does not understand why you saw the need to edit their code.
  * letting the original developer know why your changing their code can be beneficial to them if you have found a better way to do something, or if you found a problem with their code. Helping each other to become better programmers is a hopeful side effect of this project and one I feel we should strive to attain.
  * Letting the original developer know why you are editing their work is also just about being respectful to them and their code.