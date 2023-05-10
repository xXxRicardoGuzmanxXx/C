# C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_STR_LEN 100

int validar_actividad(char *actividad, char *fecha_validacion) {
    char *fecha = strtok(actividad, ",");
    char *concepto = strtok(NULL, ",");

    if (fecha == NULL || concepto == NULL) {
        return 0;
    }

    if (strcmp(fecha, fecha_validacion) == 0) {
        printf("Actividad validada: %s\n", actividad);
        return 1;
    } else {
        printf("Actividad no validada: %s\n", actividad);
        return 0;
    }
}

int main() {
    int n;
    char fecha_validacion[MAX_STR_LEN];
    char **actividades;

    printf("Ingrese la fecha de validación en formato dd/mm/yyyy: ");
    scanf("%s", fecha_validacion);

    printf("Ingrese la cantidad de actividades a validar: ");
    scanf("%d", &n);

    actividades = (char **)malloc(n * sizeof(char *));

    for (int i = 0; i < n; i++) {
        actividades[i] = (char *)malloc(MAX_STR_LEN * sizeof(char));
        printf("Ingrese la actividad #%d en formato dd/mm/yyyy,concepto: ", i + 1);
        scanf("%s", actividades[i]);
    }

    FILE *archivo = fopen("actividades.txt", "w");

    for (int i = 0; i < n; i++) {
        int actividad_valida = validar_actividad(actividades[i], fecha_validacion);
        fprintf(archivo, "%s - %s\n", actividades[i], actividad_valida ? "Actividad validada" : "Actividad no validada");
    }

    fclose(archivo);

    for (int i = 0; i < n; i++) {
        free(actividades[i]);
    }

    free(actividades);

    return 0;
}
