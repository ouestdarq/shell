#!/bin/bash

if test -t 1; then
    ncolors=$(tput colors)
    if test -n "$ncolors" && test "$ncolors" -ge 8; then
        GREEN="$(tput setaf 82)"
        RED="$(tput setaf 160)"
        YELLOW="$(tput setaf 226)"
        NC="$(tput sgr0)"
    fi
fi

function display_help {
	printf "%s\n"	"Usage: rmcaches [OPTIONS...]"
	printf "\n"
	printf "%s\n"	"Options:"
	printf "%s\n"	"  -p	clear page cache"
	printf "%s\n"	"  -m	clear dentries and inodes"
	printf "%s\n"	"  -h	display help message"
}

function drop_caches {
	PAGE_MESSAGE="page"
	MEMORY_MESSAGE="dentries/inodes"
	if [ $# -gt 0 ]; then
		if [ "$1" == "1" ]; then
			LOG=${PAGE_MESSAGE}
		elif [ "$1" == "2" ]; then
			LOG=${MEMORY_MESSAGE}
		elif [ "$1" == "3" ]; then
			LOG="${PAGE_MESSAGE} ${MEMORY_MESSAGE}"
		else
			printf "%s\n"	"${RED}ERROR:${NC} undefined drop_caches argument: ${1}" 1>&2
			exit 1
		fi

		sync; echo ${1} > /proc/sys/vm/drop_caches	&& 
		for val in $LOG; do
			printf "%s\n"   "${GREEN}Cleared:${NC} ${val}."
		done
	else
		exit 1
	fi
}

PAGE=0
MEMORY=0

while getopts ":pmh" o; do
    case "${o}" in
        p)
            PAGE=1
            ;;
        m)
            MEMORY=2
            ;;
        h)
            display_help
			exit 0
            ;;
		*)
			printf "%s\n" "${RED}rmcaches:${NC} invalid option -- '${OPTARG}'" 1>&2
			exit 1
			;;
    esac
done

shift $((OPTIND-1))

if [ $# -eq 0 ]; then
	TARGET=$(($PAGE + $MEMORY))
	if [ $TARGET == 0 ]; then
		printf "%s\n" "${RED}ERROR:${NC} At least one option should be present." 1>&2
		exit 1
	fi
	drop_caches $TARGET
else
	printf "%s\n" "${RED}ERROR:${NC} this script accepts no arguments. (${#} received)" 1>&2
	exit 1
fi
