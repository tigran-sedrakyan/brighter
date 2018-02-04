OPTIND=1

usage="Usage:
$(basename "$0") [OPTIONS] -o <output_device> -- change external monitor brightness

OPTIONS:
	-c get current level of brightness
	-i increase brightness by given percent
	-d decrease brightness by given percent
	-h show this help"

current=$(python -c "print (round(`xrandr --verbose | grep -m 1 -i brightness | cut -f2 -d ' '`, 2))")
new=""
output=""

if [ $# -eq 0 ]; then
    echo "No arguments provided"
    echo "$usage"
    exit 1
fi

while getopts hci:d:o: opt; do
    case "$opt" in
    i)
    	if [ "$(python -c "print(type($OPTARG) == int)")" == 'True' ]
    	then
    		new=$(python -c "print ($current + $OPTARG/100)")
    	fi
    	;;
    d)
		if [ "$(python -c "print(type($OPTARG) == int)")" == 'True' ]
    	then
    		new=$(python -c "print ($current - $OPTARG/100)")
    	fi
    	;;
    o)
		output=$OPTARG
        ;;
    c)	
		echo "$(python -c "print(round($current * 100), '%')")"
		exit 1
		;;
	h)
		echo "$usage"
		exit 1
		;;
    (*)	echo "$usage"
		exit 1
		;;
    esac
done

if ! [ -z $new ] && ! [ -z $output ]
	new=$(python -c "print (round($new, 2))")
then
	if [ $(python -c "print ($new <= 1 and $new >= 0.05)") == 'True' ]
	then
		$(xrandr --output $output --brightness "$new")
		echo "Brightness: $(python -c "print(round($new*100))") %"
		$(kdialog --title Brightness --passivepopup "$(python -c "print(int($new * 100), '%')")" 1)
		#$(notify-send --expire-time=1 "Brightness" "$(python -c "print(round($new * 100), '%')")")
	else
		echo "Resulting brightness shouldn't drop below 5% or go over 100% (current brightness is $(python -c "print(round($current*100))") %)"
	fi
elif [ -z $output ]
then
	echo "Make sure that output is specified"
fi
