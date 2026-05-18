---
status: new
---

# Run Java Application with a Specific Path in SysVinit on RHEL 6 (Spring Boot Fully Executable JAR)
!!! note
    In this case, the configuration is based on the code in the existing JAR file

1. Check the JAR file to ensure the configuration includes a configuration file setting, for example:
    ```bash hl_lines="2"
    # Source any config file
    configfile="$(basename "${jarfile%.*}.conf")"

    # Initialize CONF_FOLDER location defaulting to jarfolder
    [[ -z "$CONF_FOLDER" ]] && CONF_FOLDER="${jarfolder}"

    # shellcheck source=/dev/null
    [[ -r "${CONF_FOLDER}/${configfile}" ]] && source "${CONF_FOLDER}/${configfile}"
    ```

2. Add environment variable configuration settings for Java
    ```bash
    cat <<EOF > /path/to/directory/<jar-file-name>.conf
    JAVA_HOME=/path/to/java-home
    PATH=/path/to/java-home/bin:$PATH
    EOF
    ```
3. Create symlink from JAR file to SysVinit
    ```bash
    sudo ln -s <jar-file-name>.jar /etc/init.d/<service-name>
    ```
4. Run the service
    ```bash
    sudo /etc/init.d/<service-name> start
    ```
