Init script for Sidekiq
___

Create the file /etc/init.d/sidekiq with the following content:

    #!/bin/bash
    # sidekiq    Init script for Sidekiq
    # chkconfig: 345 100 75
    #
    # Description: Starts and Stops Sidekiq message processor for Stratus application.
    #
    # User-specified exit parameters used in this script:
    #
    # Exit Code 5 - Incorrect User ID
    # Exit Code 6 - Directory not found

    # You will need to modify these
    APP="sidekiq"
    AS_USER="webapp"
    APP_DIR="/home/webapp/app-name/current"

    LOG_FILE="$APP_DIR/log/sidekiq.log"
    LOCK_FILE="$APP_DIR/tmp/pids/${APP}-lock"
    PID_FILE="$APP_DIR/tmp/pids/${APP}.pid"
    SIDEKIQ="sidekiq"
    APP_ENV="production"
    BUNDLE="bundle"

    START_CMD="$BUNDLE exec $SIDEKIQ -C /home/webapp/app-name/config/sidekiq.yml -e $APP_ENV -P $PID_FILE"
    CMD="cd ${APP_DIR}; ${START_CMD} >> ${LOG_FILE} 2>&1 &"

    RETVAL=0

    start() {

      status
      if [ $? -eq 1 ]; then

        [ `id -u` == '0' ] || (echo "$SIDEKIQ runs as root only .."; exit 5)
        [ -d $APP_DIR ] || (echo "$APP_DIR not found!.. Exiting"; exit 6)
        cd $APP_DIR
        echo "Starting $SIDEKIQ message processor .. "

        su -c "$CMD" - $AS_USER

        RETVAL=$?
        #Sleeping for 8 seconds for process to be precisely visible in process table - See status ()
        sleep 8
        [ $RETVAL -eq 0 ] && touch $LOCK_FILE
        return $RETVAL
      else
        echo "$SIDEKIQ message processor is already running .. "
      fi

    }

    stop() {

        echo "Stopping $SIDEKIQ message processor .."
        SIG="INT"
        kill -$SIG `cat  $PID_FILE`
        RETVAL=$?
        [ $RETVAL -eq 0 ] && rm -f $LOCK_FILE && rm -f $PID_FILE
        sleep 8
        return $RETVAL
    }

    status() {

      ps -ef | grep 'sidekiq' | grep $(cat $PID_FILE) | grep -v grep
      return $?
    }

    case "$1" in
        start)
            start
            ;;
        stop)
            stop
            ;;
        status)
            status

            if [ $? -eq 0 ]; then
                 echo "$SIDEKIQ message processor is running .."
                 RETVAL=0
             else
                 echo "$SIDEKIQ message processor is stopped .."
                 RETVAL=1
             fi
            ;;
        *)
            echo "Usage: $0 {start|stop|status}"
            exit 0
            ;;
    esac
    exit $RETVAL

Now, you can manage the sidekiq process with

    service sidekiq start
    service sidekiq status
    service sidekiq stop

References

 * http://cdyer.co.uk/blog/init-script-for-sidekiq-with-rbenv