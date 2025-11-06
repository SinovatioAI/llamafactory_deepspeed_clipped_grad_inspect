# 项目说明

1. 打印裁剪后梯度范数涉及到：
   1. 在deepspeed源码中，梯度更新之后重新计算一次总的梯度范数
   2. 在transformer库中，修改训练类，以获取裁剪后的梯度范数
   3. 在llamafactory源码中，修改日志记录的回调函数，把裁剪前后的梯度范数存放到trainer_log.jsonl文件中
2. 目前的修改支持以命令行的方式，在启用deepspeed zero1或者zero2的情况下（不启用CPU offload），向终端以及日志文件输出裁剪前后的梯度范数
3. 相关需要修改的文件在llamafactory镜像中的位置：
   1. callbacks.py：/app/src/llamafactory/train/callbacks.py
   2. engine.py：/opt/conda/lib/python3.11/site-packages/deepspeed/runtime/engine.py
   3. stage_1_and_2.py：/opt/conda/lib/python3.11/site-packages/deepspeed/runtime/zero/stage_1_and_2.py
   4. trainer.py：/opt/conda/lib/python3.11/site-packages/transformers/trainer.py
