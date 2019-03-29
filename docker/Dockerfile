FROM taichih/ros:cuda9.2-cudnn7-devel-ubuntu16.04
SHELL ["bash", "-c"]

MAINTAINER taichiH higashide@jsk.imi.i.u-tokyo.ac.jp

ENV ROS_DISTRO kinetic
ENV WORKSPACE /ros_ws

# instal deps
RUN pip install scikit-image

# create catkin workspace
RUN mkdir -p ${WORKSPACE}/src
WORKDIR ${WORKSPACE}
RUN wstool init ${WORKSPACE}/src
RUN cd ${WORKSPACE}/src/; git clone https://github.com/iKrishneel/dcasn; cd ${WORKSPACE}

# catkin build
WORKDIR ${WORKSPACE}
RUN source /opt/ros/${ROS_DISTRO}/setup.bash \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/

RUN source /opt/ros/${ROS_DISTRO}/setup.bash \
    && catkin config --init
RUN source /opt/ros/${ROS_DISTRO}/setup.bash \
    && catkin build

RUN touch /root/.bashrc && \
    echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> /root/.bashrc && \
    echo "rossetip\n" >> /root/.bsahrc && \
    echo "rossetmaster localhost\n"

COPY ./ros_entrypoint.sh /

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]