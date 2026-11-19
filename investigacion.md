# Investigación de Incidencia - Servidor Web

**Alumno:** [Tu Nombre y Apellidos]  
**Módulo:** Sistemas Microinformáticos y Redes (SMR)  

---

## 1. Problema detectado
El responsable de seguridad ha detectado que el cortafuegos del servidor web (`servidor.conf`) se encuentra desactivado (`firewall=no`), comprometiendo la seguridad de la infraestructura.

---

## 2. Comandos de Git utilizados en la investigación
- `git log --oneline`: Para visualizar el historial resumido de commits y localizar el flujo de cambios.
- `git log -p`: Para inspeccionar las líneas exactas modificadas en cada commit.
- `git show <ID_COMMIT>`: Para analizar los cambios introducidos en el commit sospechoso.
- `git diff <ID1> <ID2>`: Para comparar el estado del archivo de configuración entre diferentes versiones.

---

## 3. Identificación del error
- **Commit incorrecto:** El commit con mensaje *"Actualizacion de seguridad"* (Commit 3).
- **Explicación:** Mediante `git log -p` se comprobó que en este commit específico se modificó la línea `firewall=si` por `firewall=no`, desactivando la seguridad del sistema de forma imprevista.

---

## 4. Recuperación mediante `git checkout`
Al ejecutar `git checkout <ID_COMMIT_ANTERIOR> -- servidor.conf`:

1. **¿Qué versión del archivo has recuperado?**  
   Se recuperó la versión del archivo correspondiente al commit anterior al error (cuando el firewall seguía activo y el puerto era el inicial).
2. **¿Se han perdido temporalmente las modificaciones posteriores?**  
   Sí, en el directorio de trabajo el archivo revirtió temporalmente a un estado anterior, ocultando los cambios posteriores (como el puerto 8080, `inicio.html` y `max_conexiones=100`).
3. **¿Han desaparecido los commits posteriores del historial?**  
   No. El historial de commits en el repositorio permanece intacto; únicamente se modificó la versión actual del archivo en el directorio de trabajo.
4. **¿Por qué no conviene utilizar directamente esta versión antigua como solución definitiva?**  
   Porque revertiría por completo a un estado obsoleto, haciendo que la empresa perdiera mejoras y modificaciones necesarias y válidas implementadas posteriormente (el puerto 8080, la nueva página principal `inicio.html` y el límite de conexiones).

---

## 5. Solución definitiva
Para solucionar el problema manteniendo todas las modificaciones válidas:
1. Se restauró el archivo a su estado más reciente: `git restore --source=HEAD --staged --worktree servidor.conf`
2. Se modificó manualmente el archivo para volver a activar el cortafuegos (`firewall=si`).
3. Se registró la corrección mediante un nuevo commit: `git commit -m "Correccion de configuracion de seguridad"`

---

## 6. Utilidad de Git en la administración de sistemas
Git es una herramienta fundamental para los administradores de sistemas porque permite:
- **Trazabilidad:** Saber exactamente qué usuario o técnico hizo un cambio, cuándo lo hizo y por qué motivo (gracias a los mensajes de commit).
- **Reversibilidad:** Deshacer errores o recuperar configuraciones estables de forma rápida y segura sin recurrir a copias de seguridad manuales caóticas (`servidor_backup_final_2.conf`).
- **Auditoría:** Facilitar la investigación de fallos de seguridad o interrupciones del servicio en entornos de producción.