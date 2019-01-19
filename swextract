#!/bin/sh

help() {
        echo "Usage: ./swextract.sh [idb absolute path] [sw absolute path] [outpath]"
        echo "Example: ./swextract.sh /home/user/test/eoe.idb /home/user/test/eoe.sw /home/user/irixroot"
        exit 0
}

if [ $# -eq 0 ]; then
	help
fi

idbfile=$1
swfile=$2
outpath=$3

cat ${idbfile} | while read line; do
        if echo ${line} | grep -Eq "^d\ "; then
                dir=$(echo ${line} | awk '{print $5}')
                mkdir -p ${outpath}/${dir}
        fi
        filename=$(echo ${line} | awk '{print $5}' | rev | awk -F/ '{print $1}' | rev)
        path=$(echo ${line} | awk '{print $5}' | rev | cut -d/ -f2- | rev)
        fullpath=$(echo ${line} | awk '{print $5}')
        strlen=$(echo -n ${fullpath} | wc -c)
        cmpsize=$(echo ${line} | awk -F 'cmpsize\\(' '{print $2}' | awk -F ')' '{print $1}')
        offset=$(echo ${line} | awk -F 'off\\(' '{print $2}' | awk -F ')' '{print $1}')


        echo "Extracting ${filename} from ${idbfile} to ${outpath}/${fullpath}"
        dd if=${swfile} of=${outpath}/${fullpath}.Z skip=$((${offset}+2+${strlen})) count=${cmpsize} bs=1 2>/dev/null
        cd ${path}
        echo "Uncompressing ${fullpath}.Z"
        uncompress ${filename}.Z 2>/dev/null
        rm -f ${filename}.Z
        cd ${outpath}
#       echo ====================
#       echo $filename
#       echo $path
#       echo $path/$filename
#       echo $strlenlong
#       echo strlen $strlen
#       echo cmpsize $cmpsize
#       echo offset $offset
#       echo $outpath
#       echo $idbfile
#       echo $swfile
#       echo $wdir
done
exit 0
