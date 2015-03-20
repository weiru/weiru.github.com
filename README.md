# weiru.github.com

package wjtaia.test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class ShortestPath
{
    final int INFINITE = 9999;
    final static char WALL = '#';
    final char BOX = '@';
    final char START = 'S';
    final char END = 'G';
    List<Point> p_list = new ArrayList<Point>();

    /**
     *
     */
    public ShortestPath(final int x, final int y)
    {
        final Point p = new Point(1, 4);
        p_list.add(p);
        final Point from = new Point(x, y);
        p.findSP(x, y, 0, from);
    }

    private class Point
    {
        Map<Point, Integer> sp = new HashMap<Point, Integer>();
        int x = 0;
        int y = 0;

        /**
         *
         */
        public Point(final int x, final int y)
        {
            this.x = x;
            this.y = y;
        }

        public void setSP(final Point p, final int count)
        {
            if( !sp.containsKey(p) || sp.get(p) > count )
            {
                System.out.println("setSP()  x:" + p.x + " y:" + p.y + " ct:" + count);
                sp.put(p, count);
            }
        }

        public void findSP(final int from_x, final int from_y, final int count, final Point p)
        {
            System.out.println("x:" + from_x + " y:" + from_y + " ct:" + count);
            if( from_x == x && from_y == y )
            {
                setSP(p, count);
                return;
            }
            if( hitWall(from_x, from_y) )
            {
                return;
            }
            findSP(from_x + 1, from_y, count + 1, p);
            findSP(from_x, from_y + 1, count + 1, p);
        }
    }

    static int[][] map = new int[100][100];

    public boolean hitWall(final int x, final int y)
    {
        if( map[y][x] == -1 )
        {
            System.out.println("hitWall!");
            return true;
        }
        return false;
    }


    /**
     * @param args
     */
    public static void main(final String[] args)
    {
        try
        {
            final BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            final String input = br.readLine();
            final String[] wh = input.split(" ");
            final int width = Integer.valueOf(wh[0]);
            final int height = Integer.valueOf(wh[1]);

            for ( int y = 0 ; y < height ; y++ )
            {
                final char[] line = new char[width + 2];
                br.read(line);
                System.out.println(line);
                for ( int x = 0 ; x < width ; x++ )
                {
                    final char c = line[x];
                    if( c == WALL )
                    {
                        map[y][x] = -1;
                        System.out.println("(" + x + "," + y + ")");
                    }
                }
            }
        }
        catch( final IOException io )
        {
            io.printStackTrace();
        }

        final ShortestPath q1 = new ShortestPath(1, 1);

        for ( final Map.Entry<Point, Integer> entry : q1.p_list.get(0).sp.entrySet() )
        {
            System.out.println("(" + entry.getKey().x + ", " + entry.getKey().y + ") sp:" + entry.getValue());
        }

    }

}
