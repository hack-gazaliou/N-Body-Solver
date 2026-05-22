cmake_minimum_required(VERSION 3.15)
project(NBodySolver VERSION 1.0 LANGUAGES CXX C)

# 1. Configuration du C++
# On utilise C++17 (très pratique pour le multithreading CPU de ta V2)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 2. Recherche des librairies système
# On demande à CMake de trouver OpenGL et les Threads (pour la V2) sur ton OS
find_package(OpenGL REQUIRED)
find_package(Threads REQUIRED)

# Note : Si tu installes GLFW via ton OS (ex: apt install libglfw3-dev), décommente la ligne ci-dessous.
# find_package(glfw3 REQUIRED)

# 3. Récupération de tous tes fichiers sources
# GLOB_RECURSE va chercher automatiquement tous les .cpp dans src/ et ses sous-dossiers
file(GLOB_RECURSE SOURCES "src/*.cpp")

# Si tu utilises GLAD (qui a un fichier glad.c), on l'ajoute aux sources à compiler
# Décommente cette ligne si tu as mis GLAD dans external/ :
# list(APPEND SOURCES "external/glad/src/glad.c")

# 4. Création de l'exécutable
add_executable(nbody_solver ${SOURCES})

# 5. Indiquer où se trouvent les fichiers d'en-tête (.h / .hpp)
target_include_directories(nbody_solver PRIVATE 
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_SOURCE_DIR}/external
    # Si GLAD ou GLFW sont dans external, ajoute leurs sous-dossiers include :
    # ${CMAKE_SOURCE_DIR}/external/glad/include
    # ${CMAKE_SOURCE_DIR}/external/glfw/include
)

# 6. Lier les librairies à ton exécutable
target_link_libraries(nbody_solver PRIVATE 
    OpenGL::GL 
    Threads::Threads
    # glfw  # Décommente si tu utilises le glfw du système
)

# 7. L'astuce pour les Shaders
# Cette commande copie automatiquement ton dossier "shaders/" à côté de ton exécutable 
# dans le dossier "build/". Ainsi, ton programme trouvera toujours les .vert et .frag.
file(COPY ${CMAKE_SOURCE_DIR}/shaders DESTINATION ${CMAKE_BINARY_DIR})