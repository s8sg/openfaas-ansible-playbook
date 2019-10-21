FROM openfaas/classic-watchdog:0.13.4 as watchdog

FROM cmccarth/alpine-snow:latest

USER root

RUN mkdir -p /app

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

COPY function/definition  /definition

RUN source /definition; apk --no-cache add ca-certificates git ${additional_packages}

# Check if mandetory args not provided
RUN source /definition; test -n "$playbook_url" && test -n "$playbook" \
    && test -n "$inventory_url" && test -n "$inventory"

# Add non root user
RUN addgroup -S app && adduser app -S -G app
RUN chown app /home/app

WORKDIR /home/app

USER app

RUN source /definition; git clone $playbook_url project
RUN source /definition; git clone $inventory_url inventory

WORKDIR /home/app/project
COPY execute_playbook execute_playbook

ENV fprocess="sh /home/app/project/execute_playbook"

EXPOSE 8080
EXPOSE 31112
EXPOSE 22

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]

ENV write_debug="false"
ENV read_timeout="0"
ENV write_timeout="0"
