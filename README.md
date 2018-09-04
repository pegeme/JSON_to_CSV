# JSON_to_CSV
Read JSON file and output three specific information in a CSV file.


import csv
import json
import re

SEARCH_TIMESTAMP = '__REALTIME_TIMESTAMP'
SEARCH_MESSAGE = 'MESSAGE'
SEARCH_TRACE_ID = 'smarthub.traceId'
SEARCH_PERFORMANCE_MARKER = 'PERFORMANCE_MARKER'

source_file = open('Client_log_jason_ALL.json', 'r')
output_file = open('test.csv', "w+")

for data in json.load(source_file):
        # Setze Werte zurück
        timeStamp = ''
        traceId = ''
        performanceMarker = ''

       ## Suche CSV Werte
        #
        if SEARCH_TIMESTAMP in data:
                timeStamp = data[SEARCH_TIMESTAMP]
        if SEARCH_TRACE_ID in data:
                traceId = data[SEARCH_TRACE_ID]
        if SEARCH_MESSAGE in data:
                message = data[SEARCH_MESSAGE]
                if message.find(SEARCH_PERFORMANCE_MARKER) != -1:
                        # "I0731 11:05:01.533710   532 vocond.cc:129] PERFORMANCE_MARKER: WuW detected = hallo_magenta"
                        matchOBj = re.search(SEARCH_PERFORMANCE_MARKER + ': (.+?)$', message)
                        if matchOBj:
                                performanceMarker = matchOBj.group(1)
        #
        ## Schreibe CSV
        # Ausgabe CSV-Struktur PERFORMANCE_MARKER, TRACE_ID_, REALTIME_TIMESTAMP
        # Nur schreiben wenn PerformanceMarker vorhanden ist?
        if performanceMarker != '':
            output = 'PERFORMANCE_MARKER;"{}";TRACE_ID;{};REALTIME_TIMESTAMP;{}\n'.format(performanceMarker, traceId, timeStamp)
            print(output)
            output_file.write(output)


source_file.close()
output_file.close()
