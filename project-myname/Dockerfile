# super small image for the container
FROM alpine

# because our get-last-name is in python
RUN apk update && apk add python

# add a default first-last name file that could be over-written by docker volumes
COPY name.txt /usr/local/name.txt

COPY scripts/get-first-name /usr/local/bin/get-first-name
COPY scripts/get-last-name /usr/local/bin/get-last-name
COPY scripts/get-name /usr/local/bin/get-name

RUN chmod +x /usr/local/bin/get-first-name /usr/local/bin/get-last-name /usr/local/bin/get-name

CMD ["/usr/local/bin/get-name"]
