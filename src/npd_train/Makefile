
LINK = libtool --mode=link --tag=CXX g++

OPENCV_BRANCH:=3.2.0
OPENCV_SRC :=/home/thomas/opencv/${OPENCV_BRANCH}
OPENCV_OUT:=/home/thomas/opencv/build-${OPENCV_BRANCH}/x86_64/debug

C := gcc
CXX := g++

AR      := ar
ARFLAGS := crv
RANLIB  := ranlib
SHARED  := -shared
SRC_DIR := 
LIBCXXFLAGS := -fno-strict-aliasing -fPIC  -fopenmp -fpermissive
LIBCFLAGS := -fPIC -fopenmp

CFLAGS = -Wall ${LIBCFLAGS}
CXXFLAGS = -Wall ${LIBCXXFLAGS}

DLIB = 0

# Compiler switches. Default -g.
ifeq ($(RELEASE), 1)
    CFLAGS += -O3 -DNDEBUG -DSHMEM -DDLIB_PNG_SUPPORT -DDLIB_JPEG_SUPPORT
    CXXFLAGS += -O3 -DNDEBUG -DSHMEM -DDLIB_PNG_SUPPORT -DDLIB_JPEG_SUPPORT
    DLIBSOURCE = 
else
    CFLAGS +=  -DDEBUG  -pg -g3 -DSHMEM -DDLIB_PNG_SUPPORT -DDLIB_JPEG_SUPPORT
    CXXFLAGS +=  -DDEBUG  -pg -g3 -DSHMEM -DDLIB_PNG_SUPPORT -DDLIB_JPEG_SUPPORT
    ifeq ($(DLIB), 1)
        DLIBSOURCE = ../../3rdparty/dlib/dlib/all/source.cpp
    endif
endif

# Compiler switches. Defult no multi thread support
ifeq ($(MTHREADS), 1)
    CFLAGS += -DMULTI_THREAD_SUPPORT
    CXXFLAGS += -DMULTI_THREAD_SUPPORT
    EXTRALIBS += -pthread
endif

INC := \
	-I. \
	-I../../include/ \
	-I${OPENCV_SRC}/include \
	-I${OPENCV_SRC}/include/opencv \
	-I${OPENCV_SRC}/modules/core/include \
	-I${OPENCV_SRC}/modules/calib3d/include \
	-I${OPENCV_SRC}/modules/features2d/include \
	-I${OPENCV_SRC}/modules/flann/include \
	-I${OPENCV_SRC}/modules/highgui/include \
	-I${OPENCV_SRC}/modules/imgcodecs/include \
	-I${OPENCV_SRC}/modules/video/include \
	-I${OPENCV_SRC}/modules/videoio/include \
	-I${OPENCV_SRC}/modules/imgproc/include \
	-I${OPENCV_SRC}/modules/ml/include \
	-I${OPENCV_SRC}/modules/objdetect/include \
	-I${OPENCV_SRC}/modules/photo/include \
	-I${OPENCV_SRC}/modules/shape/include \
	-I${OPENCV_SRC}/modules/stitching/include \
	-I${OPENCV_SRC}/modules/superres/include \
	-I${OPENCV_SRC}/modules/videostab/include \
	-I${OPENCV_OUT} \
	-I/usr/local/include/

CFLAGS += $(INC)
CXXFLAGS += $(INC)

LDDIR := \
	-L../../lib/ \
	-L${OPENCV_OUT}/lib/ \
	-L/usr/local/lib/

OPENCVLIBS= -lopencv_highgui -lopencv_core \
			-lopencv_imgproc  \
			-lopencv_flann \
			-lopencv_calib3d -lopencv_ml \
			-lopencv_video  -lopencv_videoio \
			-lopencv_objdetect -lopencv_photo \
			-lopencv_imgcodecs \
			-lopencv_features2d

#			-lopencv_contrib -lopencv_legacy
#			-lopencv_gpu
#			-lopencv_ocl -lopencv_nonfree \


ifeq ($(DLIB), 1)
    DLIBLIBS = -ldlib 
else
    DLIBLIBS = 
endif

EXTRALIBS= $(DLIBLIBS) $(OPENCVLIBS) -lgomp

OBJS = $(patsubst %.cpp,%.o,$(wildcard *.cpp))

.PHONY: all 

all:  $(OBJS) trainNPD 

trainNPD: $(OBJS) 
	$(CXX) $(CXXFLAGS) $(LDDIR) -o ../../bin/$@ $^ $(EXTRALIBS) -fopenmp -lpthread


clean:
	rm -rf *.o
