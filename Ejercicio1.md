#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
void invertirCadena(char *cadena) {
    int i, j;
    char temp;
    int length = strlen(cadena);
    for (i = 0, j = length - 1; i < j; i++, j--) {
        temp = cadena[i];
        cadena[i] = cadena[j];
        cadena[j] = temp;
    }
}
void contarPalabrasVocalesConsonantesEspacios(const char *cadena, int *numPalabras, int *numVocales, int *numConsonantes, int *numEspacios) {
    *numPalabras = 0;
    *numVocales = 0;
    *numConsonantes = 0;
    *numEspacios = 0;
    int length = strlen(cadena);
    int i;
    int enPalabra = 0; // Flag para indicar si estamos dentro de una palabra
    for (i = 0; i < length; i++) {
        if (cadena[i] == ' ') {
            *numEspacios += 1;
            enPalabra = 0;
        } else {
            if (!enPalabra) {
                *numPalabras += 1;
                enPalabra = 1;
            }
            char c = tolower(cadena[i]);
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u')
                *numVocales += 1;
            else if (isalpha(c))
                *numConsonantes += 1;
        }
    }
}
int main() {
    int n;
    printf("Ingrese la cantidad de cadenas: ");
    scanf("%d", &n);
    getchar();
    FILE *archivo = fopen("resultado.txt", "w");
    if (archivo == NULL) {
        printf("No se pudo abrir el archivo.\n");
        return 1;
    }
    int i;
    for (i = 0; i < n; i++) {
        char *cadena = NULL;
        size_t capacidad = 0;
        printf("Ingrese una cadena de caracteres: ");
        getline(&cadena, &capacidad, stdin);
        cadena[strcspn(cadena, "\n")] = '\0';
        invertirCadena(cadena);
        int numPalabras, numVocales, numConsonantes, numEspacios;
        contarPalabrasVocalesConsonantesEspacios(cadena, &numPalabras, &numVocales, &numConsonantes, &numEspacios);
        fprintf(archivo, "%s\n", cadena);
        fprintf(archivo, "Tiene %d espacios, %d vocales y %d consonantes.\n", numEspacios, numVocales, numConsonantes);
        fprintf(archivo, "La cadena contiene %d palabras.\n", numPalabras);
    free(cadena);
}

fclose(archivo);

return 0;
}
