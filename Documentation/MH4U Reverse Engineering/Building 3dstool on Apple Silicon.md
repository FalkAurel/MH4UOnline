If you have issues related to building 3dstoolkit, you might want to consider the following steps:

1. Verify that OpenSSL is installed by running:

    ```bash
    brew install openssl
    ```

   If OpenSSL is already installed, you might receive a message similar to the following:

    ```plaintext
    Warning: openssl@3 3.2.1 is already installed and up-to-date.
    To reinstall 3.2.1, run:
    brew reinstall openssl@3
    ```

2. Install the repository, create a folder named build, and move into it:

    ```bash
    git clone https://github.com/dnasdw/3dstool && cd 3dstool && mkdir build && cd build
    ```

3. Edit the **CMakeList.txt** file in `../3dstool/src` by running this command: 

    *Note: In this case, I'm using NeoVim, but any editor will do.*
    
    ```bash
    nvim ../src/CMakeLists.txt
    ```

4. Insert at **line 33**, **under the `else()` statement**. It should look like this after you have finished editing it:

    ```plaintext
    else()
      find_package(OpenSSL REQUIRED)
      
      if (OPENSSL_FOUND)
        include_directories(${OPENSSL_INCLUDE_DIR})
        target_link_libraries(${PROJECT_NAME} curl ${OPENSSL_LIBRARIES})
      else()
        message(FATAL_ERROR "OpenSSL not found!")
      endif()
    ```

5. Finish the build process as described in the repository.
