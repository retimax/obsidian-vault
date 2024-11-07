### Karol René Rivas Díaz
# Datos
- En 2024 se detectan por trimestre un 26% más de CVEs (**Common Vulnerabilities and Exposures**) que en 2023, se estima que el aproximado total de estas para fin de año sean 32K.
- Mas de 80% de vulnerabilidades explotadas tienen varios años de existencia, lo que indica hasta cierto punto el desinterés que se tiene aun en estos años por la ciberseguridad.
- Casi 32% de las empresas sufrieron incidentes de seguridad.
- Solo el 27% considera la ciberseguridad como un tema crítico.

# Riesgos
- Uno de los riesgos mas latentes en latinoamerica respecto a la ciberseguridad sigue siendo la falta de interés de las empresas y de los empleados de dichas, lo que fácilmente en un futuro puede desencadenar en un vector de entrada para un atacante debido a la imprudencia de el personal.
- Otro riesgo visto en el webinar es la falta de inventario, ya que el 90% de las empresas no lo tienen, 70% tienen **35%** más equipos de los que creían y la gran mayoria nunca han analizado la red completa, solo:
	- ***IPs públicas***
	- ***WebApps (habitualmente ya en producción)***
	- ***Servidores***

# Automatización
La herramienta que nos mostraron es un escaner de vulnerabilidades llamado ***Hacknoid*** el cual facilita la recolección de datos acerca de las vulnerabilidades de una red empresarial, esto permite tener una vista mas amplia de los posibles vectores de entrada que podria utilizar un atacante, para así poder solucionar en cuanto antes las alertas que pongan mas en riesgo la organización.
Es importante recalcar que no necesariamente son las alertas **criticas** las que mas riesgo representan para una organización, debido a que es mas riesgosa una alerta alta o quizás media en un equipo de mayor jerarquia a una vulnerabilidad critica en uno menor, por lo que es indispensable arreglar estos desperfectos en los sistemas de manera jerarquica.
**Hacknoid** utiliza 4 criterios de priorización, los cuales son:
- **Criticidad**([CVSS FIRST](https://www.first.org/cvss/))
- **Explotabilidad**([CVSS FIRST](https://www.first.org/cvss/))
- **Exploit Prediction Scoring System**([EPSS FIRST](https://www.first.org/epss/))
- **Known Exploited Vulnerability**([KEV CISA.gov](https://www.cisa.gov/known-exploited-vulnerabilities-catalog))


# Conceptos
***Como la herramienta involucra la seguridad de la información, trazabilidad, integridad, disponibilidad, autenticidad, riesgo y vulnerabilidad.***

El tener una herramienta de análisis de vulnerabilidades involucra todos los conceptos anteriores debido a que teniendo la información de las vulnerabilidades en una empresa nos permite remediarlas con la finalidad de asegurar la seguridad de la información, valga la redundancia, lo que conlleva a mantener la integridad, disponibilidad y autenticidad de esta. Los riesgos siempre están presentes, debido a que siempre hay que tener en mente que alguien cometerá un error, siempre hay nuevas vulnerabilidades y de hecho es mas rápido explotar una vulnerabilidad recién conocida que remediarla.

El tener en cuenta estos conceptos nos ayuda para generar un mejor plan de respuesta de incidentes en colaboración con los reportes proporcionados por la herramienta de análisis de vulnerabilidades que estemos implementando. Debido a que tenemos conocimiento de los riesgos y vulnerabilidades existentes en nuestra red.

# Conclusión 
En tiempos modernos es indispensable tener una herramienta que nos automatice procesos de análisis en la red para así darle un enfoque mas eficaz a la seguridad de la información de una organización, ademas es hora de poner la importancia que merece la ciberseguridad en america latina, debido a que aun con estos datos muchas empresas no emplean las medidas necesarias para proteger la información de sus clientes, ya sea porque no le dan un asesoramiento correcto a sus empleados o porque no tienen un equipo de ciberseguridad bien fundamentado debido a la falta de personal.
