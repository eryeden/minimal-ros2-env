FROM ros2-humble-cuda-opengl

# Add colcon configurations
COPY .colcon/ /usr/local/share/colcon/colcon-home/
ENV COLCON_HOME /usr/local/share/colcon/colcon-home/development

RUN echo 'ALL ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

COPY env.sh /etc/profile.d/ade_env.sh
COPY entrypoint /ade_entrypoint
ENTRYPOINT ["/ade_entrypoint"]
CMD ["/bin/sh", "-c", "trap 'exit 147' TERM; tail -f /dev/null & while wait ${!}; [ $? -ge 128 ]; do true; done"]
