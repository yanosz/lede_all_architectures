include $(TOPDIR)/rules.mk

# Name, version and release number
# The name and version of your package are used to define the variable to point to the build directory of your package: $(PKG_BUILD_DIR)
PKG_NAME:=helloworld
PKG_VERSION:=1.0
PKG_RELEASE:=1

# Source settings (i.e. where to find the source codes)
# This is a custom variable, used below
# SOURCE_DIR:=.

include $(INCLUDE_DIR)/package.mk

# Package definition; instructs on how and where our package will appear in the overall configuration menu ('make menuconfig')
define Package/helloworld
	SECTION:=examples
	CATEGORY:=Examples
	TITLE:=Hello, All architectures!
endef

# Package description; a more verbose description on what our package does
define Package/helloworld/description
	A simple "Hello, world!" -application for all architectures.
endef

# Package preparation instructions; create the build directory and copy the source code.
# The last command is necessary to ensure our preparation instructions remain compatible with the patching system.
define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	cp Makefile $(PKG_BUILD_DIR)
endef

define Build/Compile/Default

endef
Build/Compile = $(Build/Compile/Default)

# Package install instructions; create a directory inside the package to hold our executable, and then copy the executable we built previously into the folder
define Package/helloworld/install
	$(CP) ./files/* $(1)/
endef

# This command is always the last, it uses the definitions and variables we give above in order to get the job done
$(eval $(call BuildPackage,helloworld))
